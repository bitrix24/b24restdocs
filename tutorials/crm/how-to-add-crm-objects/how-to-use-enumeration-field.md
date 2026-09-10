# How to Work with the List Field Type

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed rights is required — administrative access to the CRM section
>
> - [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) and [crm.deal.userfield.update](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-update.md) — a CRM administrator
> - [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) — a user with permission to read deals
> - [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md) — a user with permission to modify deals
> - [crm.deal.get](../../../api-reference/crm/deals/crm-deal-get.md) and [crm.deal.list](../../../api-reference/crm/deals/crm-deal-list.md) — a user with permission to read deals
> - [crm.deal.fields](../../../api-reference/crm/deals/crm-deal-fields.md) — any user

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A List field stores the identifier of the selected option rather than its text. If the Website option is selected in the field, the response contains the number `3897`, while the Website label itself is stored separately, together with the field description.

This leads to the main rule for such fields: pass the option identifier both when writing a value and when filtering. Text in a write request resets the field, and text in a filter returns unrelated deals, and in both cases without an error.

Let us walk through the scenario with deals. Create two fields: a single-value Request Source field and a multi-value Client Interests field. Fill them in a specific deal, filter deals by value, and change the set of options.

The scenario consists of five steps.

1. Create the fields using the [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) method
2. Retrieve the option identifiers using the [crm.deal.fields](../../../api-reference/crm/deals/crm-deal-fields.md) and [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) methods
3. Write the values using the [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md) method
4. Filter deals by value using the [crm.deal.list](../../../api-reference/crm/deals/crm-deal-list.md) method
5. Change the set of options using the [crm.deal.userfield.update](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-update.md) method

As a result, both fields in the deal are filled in, filtering by value returns only the required deals, and renaming an option does not break the references already stored to it.

## Before You Start

Prepare the scenario data:

- **The deal where the fields will be filled in.** You will need its `id`. The fields themselves are created for all deals at once, not for a single one
- **The set of options.** In the example, these are Website, Phone, and Partner for the single-value field and Training, Implementation, and Support for the multiple field
- **REST access.** A webhook or an application with the `crm` scope. Only a CRM administrator can create and modify fields

The examples below use deal `8421`, the field codes `UF_CRM_ENUM_ONE` and `UF_CRM_ENUM_MULTI`, and the option identifiers from the responses of the second step. In your Bitrix24, the identifiers will be different: do not hardcode them, retrieve them with the method from the second step every time.

For server-side JS examples with `B24Hook`, Node.js 18, 20, 22 or newer is required. For new projects, use version 22 or later. B24JsSDK is an ES module: save the code in an `.mjs` file or add `"type": "module"` to `package.json`. For examples with b24pysdk, Python 3.9 or newer is required.

Store the webhook URL in an environment variable and do not publish it in open code.

{% include [Example Note](../../../_includes/examples.md) %}

## 1. Create the Fields with a Set of Options

The [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) method creates a custom field for all deals. Pass the parameters:

- `FIELD_NAME` — field code. The parameter is required. If the code does not start with `UF_CRM_`, the prefix is added automatically
- `USER_TYPE_ID` — field type, `enumeration` for a list
- `MULTIPLE` — `Y` for several values, `N` for one
- `EDIT_FORM_LABEL` — field title in the deal card, specified by language
- `LIST` — array of options in the `{ "VALUE": "text" }` format. Bitrix24 assigns the identifiers itself

Without `LIST`, the field is created as well, but there is nothing to select in it: the set of options remains empty. Options can be added later in the fifth step.

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // A single helper for all calls in the scenario: the method name and parameters are passed the same way as in REST
    async function callMethod(method, params, requestId) {
        const response = await $b24.actions.v2.call.make({
            method,
            params,
            requestId
        })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }

    const sourceFieldId = await callMethod(
        'crm.deal.userfield.add',
        {
            fields: {
                FIELD_NAME: 'UF_CRM_ENUM_ONE',
                USER_TYPE_ID: 'enumeration',
                MULTIPLE: 'N',
                EDIT_FORM_LABEL: { en: 'Request source' },
                LIST: [
                    { VALUE: 'Website' },
                    { VALUE: 'Phone' },
                    { VALUE: 'Partner' }
                ]
            }
        },
        'userfield-add-source'
    )

    const interestsFieldId = await callMethod(
        'crm.deal.userfield.add',
        {
            fields: {
                FIELD_NAME: 'UF_CRM_ENUM_MULTI',
                USER_TYPE_ID: 'enumeration',
                MULTIPLE: 'Y',
                EDIT_FORM_LABEL: { en: 'Client interests' },
                LIST: [
                    { VALUE: 'Training' },
                    { VALUE: 'Implementation' },
                    { VALUE: 'Support' }
                ]
            }
        },
        'userfield-add-interests'
    )

    console.log(sourceFieldId, interestsFieldId)
    ```

- PHP

    ```php
    <?php

    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Psr\Log\NullLogger;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // A single helper for all calls in the scenario: the method name and parameters are passed the same way as in REST
    function callMethod($serviceBuilder, string $method, array $params = []): mixed
    {
        return $serviceBuilder
            ->core
            ->call($method, $params)
            ->getResponseData()
            ->getResult();
    }

    $sourceFieldId = callMethod($serviceBuilder, 'crm.deal.userfield.add', [
        'fields' => [
            'FIELD_NAME' => 'UF_CRM_ENUM_ONE',
            'USER_TYPE_ID' => 'enumeration',
            'MULTIPLE' => 'N',
            'EDIT_FORM_LABEL' => ['en' => 'Request source'],
            'LIST' => [
                ['VALUE' => 'Website'],
                ['VALUE' => 'Phone'],
                ['VALUE' => 'Partner'],
            ],
        ],
    ]);

    $interestsFieldId = callMethod($serviceBuilder, 'crm.deal.userfield.add', [
        'fields' => [
            'FIELD_NAME' => 'UF_CRM_ENUM_MULTI',
            'USER_TYPE_ID' => 'enumeration',
            'MULTIPLE' => 'Y',
            'EDIT_FORM_LABEL' => ['en' => 'Client interests'],
            'LIST' => [
                ['VALUE' => 'Training'],
                ['VALUE' => 'Implementation'],
                ['VALUE' => 'Support'],
            ],
        ],
    ]);

    print_r([$sourceFieldId, $interestsFieldId]);
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook

    bitrix_token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token=os.environ["B24_HOOK_TOKEN"],
    )
    # B24_HOOK_TOKEN = 'USER_ID/TOKEN'

    def call_method(method, params=None):
        # A single helper for all calls in the scenario: the method name and parameters are passed the same way as in REST
        return bitrix_token.call_method(
            api_method=method,
            params=params or {},
        )["result"]

    source_field_id = call_method(
        "crm.deal.userfield.add",
        {
            "fields": {
                "FIELD_NAME": "UF_CRM_ENUM_ONE",
                "USER_TYPE_ID": "enumeration",
                "MULTIPLE": "N",
                "EDIT_FORM_LABEL": {"en": "Request source"},
                "LIST": [
                    {"VALUE": "Website"},
                    {"VALUE": "Phone"},
                    {"VALUE": "Partner"},
                ],
            },
        },
    )

    interests_field_id = call_method(
        "crm.deal.userfield.add",
        {
            "fields": {
                "FIELD_NAME": "UF_CRM_ENUM_MULTI",
                "USER_TYPE_ID": "enumeration",
                "MULTIPLE": "Y",
                "EDIT_FORM_LABEL": {"en": "Client interests"},
                "LIST": [
                    {"VALUE": "Training"},
                    {"VALUE": "Implementation"},
                    {"VALUE": "Support"},
                ],
            },
        },
    )

    print(source_field_id, interests_field_id)
    ```

{% endlist %}

The response contains the field identifier, not the option identifiers:

```json
{
    "result": 6007777
}
```

The option identifiers are retrieved in the next step.

## 2. Retrieve the Option Identifiers

Two methods return the option identifiers, choose the one that fits your task.

The [crm.deal.fields](../../../api-reference/crm/deals/crm-deal-fields.md) method returns the description of all deal fields. A List field has an `items` array with `ID` and `VALUE` pairs, which is enough to map the option text to its identifier. The method is available to any user.

The [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) method returns custom fields only and the full description of the options in the `LIST` array: it contains `SORT`, the default value flag `DEF`, and `XML_ID`. Pass `filter` with `USER_TYPE_ID` or `FIELD_NAME` so that you do not have to parse all the fields.

{% list tabs %}

- JS

    ```js
    const dealFields = await callMethod('crm.deal.fields', {}, 'deal-fields')
    const sourceItems = dealFields.UF_CRM_ENUM_ONE.items

    const enumFields = await callMethod(
        'crm.deal.userfield.list',
        {
            filter: {
                USER_TYPE_ID: 'enumeration',
                FIELD_NAME: 'UF_CRM_ENUM_MULTI'
            }
        },
        'userfield-list-enum'
    )

    const interestItems = enumFields[0].LIST

    const sourceId = sourceItems.find((item) => item.VALUE === 'Website').ID
    const interestIds = interestItems
        .filter((item) => ['Training', 'Support'].includes(item.VALUE))
        .map((item) => Number(item.ID))

    console.log(sourceId, interestIds)
    ```

- PHP

    ```php
    $dealFields = callMethod($serviceBuilder, 'crm.deal.fields');
    $sourceItems = $dealFields['UF_CRM_ENUM_ONE']['items'];

    $enumFields = callMethod($serviceBuilder, 'crm.deal.userfield.list', [
        'filter' => [
            'USER_TYPE_ID' => 'enumeration',
            'FIELD_NAME' => 'UF_CRM_ENUM_MULTI',
        ],
    ]);

    $interestItems = $enumFields[0]['LIST'];

    $sourceId = 0;

    foreach ($sourceItems as $item) {
        if ($item['VALUE'] === 'Website') {
            $sourceId = (int)$item['ID'];
            break;
        }
    }

    $interestIds = [];

    foreach ($interestItems as $item) {
        if (in_array($item['VALUE'], ['Training', 'Support'], true)) {
            $interestIds[] = (int)$item['ID'];
        }
    }

    print_r([$sourceId, $interestIds]);
    ```

- Python

    ```python
    deal_fields = call_method("crm.deal.fields")
    source_items = deal_fields["UF_CRM_ENUM_ONE"]["items"]

    enum_fields = call_method(
        "crm.deal.userfield.list",
        {
            "filter": {
                "USER_TYPE_ID": "enumeration",
                "FIELD_NAME": "UF_CRM_ENUM_MULTI",
            },
        },
    )

    interest_items = enum_fields[0]["LIST"]

    source_id = int(
        next(item["ID"] for item in source_items if item["VALUE"] == "Website")
    )
    interest_ids = [
        int(item["ID"])
        for item in interest_items
        if item["VALUE"] in ("Training", "Support")
    ]

    print(source_id, interest_ids)
    ```

{% endlist %}

Abbreviated response of [crm.deal.fields](../../../api-reference/crm/deals/crm-deal-fields.md) for a single field:

```json
{
    "result": {
        "UF_CRM_ENUM_ONE": {
            "type": "enumeration",
            "isMultiple": false,
            "isRequired": false,
            "formLabel": "Request source",
            "items": [
                { "ID": "3897", "VALUE": "Website" },
                { "ID": "3899", "VALUE": "Phone" },
                { "ID": "3901", "VALUE": "Partner" }
            ]
        }
    }
}
```

Abbreviated response of [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md):

```json
{
    "result": [
        {
            "ID": "6007779",
            "FIELD_NAME": "UF_CRM_ENUM_MULTI",
            "USER_TYPE_ID": "enumeration",
            "MULTIPLE": "Y",
            "LIST": [
                { "ID": "3903", "SORT": "500", "VALUE": "Training", "DEF": "N" },
                { "ID": "3907", "SORT": "500", "VALUE": "Support", "DEF": "N" },
                { "ID": "3905", "SORT": "500", "VALUE": "Implementation", "DEF": "N" }
            ]
        }
    ],
    "total": 1
}
```

The order of options in the response matches neither the order of creation nor the order of identifiers: all options have the same `SORT`. If the order matters, set different `SORT` values at creation or sort the options in your own code.

Save the identifiers of the required options: `3897` for the single-value field and `3903` and `3907` for the multiple field. Do not carry them into your code as constants, because in another Bitrix24 the same options have different identifiers.

## 3. Write the Values to the Deal

The [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md) method writes values to the deal fields. Pass the parameters:

- `id` — deal identifier
- `fields` — object with field codes. Pass the option identifier as a number to the single-value field and an array of identifiers to the multiple field

{% note warning "" %}

Pass the option identifier rather than its text. For a string such as `"Website"`, the method returns `true`, but the field ends up containing `0`: the value is cast to a number and the text becomes zero. There is no error.

{% endnote %}

{% list tabs %}

- JS

    ```js
    await callMethod(
        'crm.deal.update',
        {
            id: 8421,
            fields: {
                UF_CRM_ENUM_ONE: Number(sourceId),
                UF_CRM_ENUM_MULTI: interestIds
            }
        },
        'deal-update-enum'
    )

    const deal = await callMethod(
        'crm.deal.get',
        { id: 8421 },
        'deal-get-enum'
    )

    console.log(deal.UF_CRM_ENUM_ONE, deal.UF_CRM_ENUM_MULTI)
    ```

- PHP

    ```php
    callMethod($serviceBuilder, 'crm.deal.update', [
        'id' => 8421,
        'fields' => [
            'UF_CRM_ENUM_ONE' => $sourceId,
            'UF_CRM_ENUM_MULTI' => $interestIds,
        ],
    ]);

    $deal = callMethod($serviceBuilder, 'crm.deal.get', ['id' => 8421]);

    print_r([$deal['UF_CRM_ENUM_ONE'], $deal['UF_CRM_ENUM_MULTI']]);
    ```

- Python

    ```python
    call_method(
        "crm.deal.update",
        {
            "id": 8421,
            "fields": {
                "UF_CRM_ENUM_ONE": source_id,
                "UF_CRM_ENUM_MULTI": interest_ids,
            },
        },
    )

    deal = call_method("crm.deal.get", {"id": 8421})

    print(deal["UF_CRM_ENUM_ONE"], deal["UF_CRM_ENUM_MULTI"])
    ```

{% endlist %}

Response of [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md):

```json
{
    "result": true
}
```

Abbreviated response of [crm.deal.get](../../../api-reference/crm/deals/crm-deal-get.md):

```json
{
    "result": {
        "ID": "8421",
        "TITLE": "List field check",
        "UF_CRM_ENUM_ONE": "3897",
        "UF_CRM_ENUM_MULTI": [3903, 3907]
    }
}
```

A single-value field comes as a string with the identifier, a multiple field as an array of numbers. The option text is not included in the response: to show Website to the user, map the identifier to `items` from the second step.

## 4. Filter Deals by Value

The [crm.deal.list](../../../api-reference/crm/deals/crm-deal-list.md) method selects deals by filter. A filter on a List field also takes the option identifier.

For a multiple field, a filter by one identifier finds all deals where that option is selected, even if other options are selected as well.

{% note warning "" %}

The option text in a filter returns neither an error nor an empty result: it returns unrelated deals. The string is cast to zero, and zero matches deals with an empty field. In a test Bitrix24, the filter `UF_CRM_ENUM_ONE: "Website"` returned 867 deals, and the field was empty in every one of them.

{% endnote %}

{% list tabs %}

- JS

    ```js
    const dealsBySource = await callMethod(
        'crm.deal.list',
        {
            filter: { UF_CRM_ENUM_ONE: Number(sourceId) },
            select: ['ID', 'TITLE', 'UF_CRM_ENUM_ONE']
        },
        'deal-list-by-source'
    )

    const dealsByInterest = await callMethod(
        'crm.deal.list',
        {
            filter: { UF_CRM_ENUM_MULTI: interestIds[0] },
            select: ['ID', 'TITLE', 'UF_CRM_ENUM_MULTI']
        },
        'deal-list-by-interest'
    )

    console.table(dealsBySource)
    console.table(dealsByInterest)
    ```

- PHP

    ```php
    $dealsBySource = callMethod($serviceBuilder, 'crm.deal.list', [
        'filter' => ['UF_CRM_ENUM_ONE' => $sourceId],
        'select' => ['ID', 'TITLE', 'UF_CRM_ENUM_ONE'],
    ]);

    $dealsByInterest = callMethod($serviceBuilder, 'crm.deal.list', [
        'filter' => ['UF_CRM_ENUM_MULTI' => $interestIds[0]],
        'select' => ['ID', 'TITLE', 'UF_CRM_ENUM_MULTI'],
    ]);

    print_r($dealsBySource);
    print_r($dealsByInterest);
    ```

- Python

    ```python
    deals_by_source = call_method(
        "crm.deal.list",
        {
            "filter": {"UF_CRM_ENUM_ONE": source_id},
            "select": ["ID", "TITLE", "UF_CRM_ENUM_ONE"],
        },
    )

    deals_by_interest = call_method(
        "crm.deal.list",
        {
            "filter": {"UF_CRM_ENUM_MULTI": interest_ids[0]},
            "select": ["ID", "TITLE", "UF_CRM_ENUM_MULTI"],
        },
    )

    print(deals_by_source)
    print(deals_by_interest)
    ```

{% endlist %}

Abbreviated response:

```json
{
    "result": [
        {
            "ID": "8421",
            "TITLE": "List field check",
            "UF_CRM_ENUM_ONE": "3897"
        }
    ],
    "total": 1
}
```

The `total` value indicates that the filter worked correctly: with a filter by text, it would be implausibly large.

## 5. Change the Set of Options

The [crm.deal.userfield.update](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-update.md) method changes the field description. The set of options is defined by the same `LIST` array, and the action depends on the keys you pass:

- `ID` and `VALUE` — rename an option. The identifier is kept, deals keep referencing it and show the new text
- `VALUE` only — add a new option, Bitrix24 assigns a new identifier to it
- `ID` and `DEL: "Y"` — delete an option

The options you do not list in `LIST` remain unchanged: there is no need to pass the full set.

{% list tabs %}

- JS

    ```js
    await callMethod(
        'crm.deal.userfield.update',
        {
            id: sourceFieldId,
            fields: {
                LIST: [
                    { ID: Number(sourceId), VALUE: 'Company Website' },
                    { VALUE: 'Advertising' }
                ]
            }
        },
        'userfield-update-rename'
    )

    const updatedFields = await callMethod(
        'crm.deal.userfield.list',
        { filter: { FIELD_NAME: 'UF_CRM_ENUM_ONE' } },
        'userfield-list-after-update'
    )

    console.table(updatedFields[0].LIST)
    ```

- PHP

    ```php
    callMethod($serviceBuilder, 'crm.deal.userfield.update', [
        'id' => $sourceFieldId,
        'fields' => [
            'LIST' => [
                ['ID' => $sourceId, 'VALUE' => 'Company Website'],
                ['VALUE' => 'Advertising'],
            ],
        ],
    ]);

    $updatedFields = callMethod($serviceBuilder, 'crm.deal.userfield.list', [
        'filter' => ['FIELD_NAME' => 'UF_CRM_ENUM_ONE'],
    ]);

    print_r($updatedFields[0]['LIST']);
    ```

- Python

    ```python
    call_method(
        "crm.deal.userfield.update",
        {
            "id": source_field_id,
            "fields": {
                "LIST": [
                    {"ID": source_id, "VALUE": "Company Website"},
                    {"VALUE": "Advertising"},
                ],
            },
        },
    )

    updated_fields = call_method(
        "crm.deal.userfield.list",
        {"filter": {"FIELD_NAME": "UF_CRM_ENUM_ONE"}},
    )

    print(updated_fields[0]["LIST"])
    ```

{% endlist %}

Abbreviated response after the rename and the addition:

```json
{
    "result": [
        { "ID": "3897", "SORT": "500", "VALUE": "Company Website", "DEF": "N" },
        { "ID": "3899", "SORT": "500", "VALUE": "Phone", "DEF": "N" },
        { "ID": "3909", "SORT": "500", "VALUE": "Advertising", "DEF": "N" },
        { "ID": "3901", "SORT": "500", "VALUE": "Partner", "DEF": "N" }
    ]
}
```

Option `3897` kept its identifier and received new text, the new Advertising option received identifier `3909`, and the rest did not change. The deal where `3897` was written now shows Company Website, so there is no need to rewrite values in deals.

To delete an option you no longer need, pass its identifier with the deletion flag:

```json
{
    "id": 6007777,
    "fields": {
        "LIST": [
            { "ID": 3901, "DEL": "Y" }
        ]
    }
}
```

## Verify the Result

The scenario is complete if the deal references the required options and filtering by value finds exactly that deal.

What to check in the responses:

- `UF_CRM_ENUM_ONE` contains a string with the option identifier rather than `"0"`
- `UF_CRM_ENUM_MULTI` contains an array of identifiers
- [crm.deal.list](../../../api-reference/crm/deals/crm-deal-list.md) with a filter by identifier returned the deal, and `total` matches the expected number
- after the option is renamed, [crm.deal.get](../../../api-reference/crm/deals/crm-deal-get.md) returns the same identifier

Open the deal card in the interface: the Request Source and Client Interests fields show the selected options.

## Errors and Troubleshooting

If the method returns an error, check the request data.

#|
|| **Code or Error Text** | **Reason and Action** ||
|| `The 'FIELD_NAME' field is not found.` | The field code is not passed to [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md). Pass `FIELD_NAME` ||
|| `ERROR_NOT_FOUND`, `The entity with ID '...' is not found.` | The identifier of a nonexistent field is passed to [crm.deal.userfield.update](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-update.md). Retrieve it with the [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) method ||
|#

The errors of this scenario are almost always silent: the method returns success while the result is wrong. Check the stored value with the [crm.deal.get](../../../api-reference/crm/deals/crm-deal-get.md) method.

- The value `"0"` means that the option text was passed to the field instead of the identifier
- There is a value but the card is empty: an identifier that is not among the field options was passed. Such an identifier is stored without an error
- The filter returned too many deals with an empty field: the option text was passed to the filter instead of the identifier
- There is nothing to select in the field: it was created without `LIST` or with an empty array. Add the options with the [crm.deal.userfield.update](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-update.md) method

To clear a single-value field, pass an empty string to it.

## Key Points

- The field stores the option identifier. The option text lives in the field description and changes independently of the deals
- Option identifiers are unique to a specific Bitrix24. They cannot be carried into code as constants: retrieve them with the [crm.deal.fields](../../../api-reference/crm/deals/crm-deal-fields.md) or [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) method before writing
- A single-value field is returned as a string, a multiple field as an array of numbers
- The order of options in the response is not guaranteed if they have the same `SORT`
- Deleting an option does not clear the deals that referenced it: the field keeps an identifier that is no longer among the options
- For other CRM objects, fields are created with the methods of the same name, for example [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md), and in a smart process with the [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) method

## Code Example

The complete scenario in a single script: it creates both fields, retrieves the option identifiers, writes the values, selects deals, and renames an option.

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const DEAL_ID = 8421

    // A single helper for all calls in the scenario: the method name and parameters are passed the same way as in REST
    async function callMethod(method, params, requestId) {
        const response = await $b24.actions.v2.call.make({ method, params, requestId })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }

    async function main() {
        const sourceFieldId = await callMethod('crm.deal.userfield.add', {
            fields: {
                FIELD_NAME: 'UF_CRM_ENUM_ONE',
                USER_TYPE_ID: 'enumeration',
                MULTIPLE: 'N',
                EDIT_FORM_LABEL: { en: 'Request source' },
                LIST: [{ VALUE: 'Website' }, { VALUE: 'Phone' }, { VALUE: 'Partner' }]
            }
        }, 'userfield-add-source')

        await callMethod('crm.deal.userfield.add', {
            fields: {
                FIELD_NAME: 'UF_CRM_ENUM_MULTI',
                USER_TYPE_ID: 'enumeration',
                MULTIPLE: 'Y',
                EDIT_FORM_LABEL: { en: 'Client interests' },
                LIST: [{ VALUE: 'Training' }, { VALUE: 'Implementation' }, { VALUE: 'Support' }]
            }
        }, 'userfield-add-interests')

        const dealFields = await callMethod('crm.deal.fields', {}, 'deal-fields')
        const sourceId = Number(dealFields.UF_CRM_ENUM_ONE.items.find((item) => item.VALUE === 'Website').ID)
        const interestIds = dealFields.UF_CRM_ENUM_MULTI.items
            .filter((item) => ['Training', 'Support'].includes(item.VALUE))
            .map((item) => Number(item.ID))

        await callMethod('crm.deal.update', {
            id: DEAL_ID,
            fields: {
                UF_CRM_ENUM_ONE: sourceId,
                UF_CRM_ENUM_MULTI: interestIds
            }
        }, 'deal-update-enum')

        const deal = await callMethod('crm.deal.get', { id: DEAL_ID }, 'deal-get-enum')

        const dealsBySource = await callMethod('crm.deal.list', {
            filter: { UF_CRM_ENUM_ONE: sourceId },
            select: ['ID', 'TITLE', 'UF_CRM_ENUM_ONE']
        }, 'deal-list-by-source')

        await callMethod('crm.deal.userfield.update', {
            id: sourceFieldId,
            fields: {
                LIST: [
                    { ID: sourceId, VALUE: 'Company Website' },
                    { VALUE: 'Advertising' }
                ]
            }
        }, 'userfield-update-rename')

        const afterRename = await callMethod('crm.deal.get', { id: DEAL_ID }, 'deal-get-after-rename')

        console.log(deal.UF_CRM_ENUM_ONE, deal.UF_CRM_ENUM_MULTI)
        console.table(dealsBySource)
        console.log(afterRename.UF_CRM_ENUM_ONE)
    }

    main().catch((error) => console.error(error.message))
    ```

- PHP

    ```php
    <?php

    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Psr\Log\NullLogger;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    const DEAL_ID = 8421;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // A single helper for all calls in the scenario: the method name and parameters are passed the same way as in REST
    function callMethod($serviceBuilder, string $method, array $params = []): mixed
    {
        return $serviceBuilder
            ->core
            ->call($method, $params)
            ->getResponseData()
            ->getResult();
    }

    $sourceFieldId = callMethod($serviceBuilder, 'crm.deal.userfield.add', [
        'fields' => [
            'FIELD_NAME' => 'UF_CRM_ENUM_ONE',
            'USER_TYPE_ID' => 'enumeration',
            'MULTIPLE' => 'N',
            'EDIT_FORM_LABEL' => ['en' => 'Request source'],
            'LIST' => [['VALUE' => 'Website'], ['VALUE' => 'Phone'], ['VALUE' => 'Partner']],
        ],
    ]);

    callMethod($serviceBuilder, 'crm.deal.userfield.add', [
        'fields' => [
            'FIELD_NAME' => 'UF_CRM_ENUM_MULTI',
            'USER_TYPE_ID' => 'enumeration',
            'MULTIPLE' => 'Y',
            'EDIT_FORM_LABEL' => ['en' => 'Client interests'],
            'LIST' => [['VALUE' => 'Training'], ['VALUE' => 'Implementation'], ['VALUE' => 'Support']],
        ],
    ]);

    $dealFields = callMethod($serviceBuilder, 'crm.deal.fields');

    $sourceId = 0;

    foreach ($dealFields['UF_CRM_ENUM_ONE']['items'] as $item) {
        if ($item['VALUE'] === 'Website') {
            $sourceId = (int)$item['ID'];
            break;
        }
    }

    $interestIds = [];

    foreach ($dealFields['UF_CRM_ENUM_MULTI']['items'] as $item) {
        if (in_array($item['VALUE'], ['Training', 'Support'], true)) {
            $interestIds[] = (int)$item['ID'];
        }
    }

    callMethod($serviceBuilder, 'crm.deal.update', [
        'id' => DEAL_ID,
        'fields' => [
            'UF_CRM_ENUM_ONE' => $sourceId,
            'UF_CRM_ENUM_MULTI' => $interestIds,
        ],
    ]);

    $deal = callMethod($serviceBuilder, 'crm.deal.get', ['id' => DEAL_ID]);

    $dealsBySource = callMethod($serviceBuilder, 'crm.deal.list', [
        'filter' => ['UF_CRM_ENUM_ONE' => $sourceId],
        'select' => ['ID', 'TITLE', 'UF_CRM_ENUM_ONE'],
    ]);

    callMethod($serviceBuilder, 'crm.deal.userfield.update', [
        'id' => $sourceFieldId,
        'fields' => [
            'LIST' => [
                ['ID' => $sourceId, 'VALUE' => 'Company Website'],
                ['VALUE' => 'Advertising'],
            ],
        ],
    ]);

    $afterRename = callMethod($serviceBuilder, 'crm.deal.get', ['id' => DEAL_ID]);

    print_r([$deal['UF_CRM_ENUM_ONE'], $deal['UF_CRM_ENUM_MULTI']]);
    print_r($dealsBySource);
    print_r($afterRename['UF_CRM_ENUM_ONE']);
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook

    DEAL_ID = 8421

    bitrix_token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token=os.environ["B24_HOOK_TOKEN"],
    )
    # B24_HOOK_TOKEN = 'USER_ID/TOKEN'

    def call_method(method, params=None):
        # A single helper for all calls in the scenario: the method name and parameters are passed the same way as in REST
        return bitrix_token.call_method(
            api_method=method,
            params=params or {},
        )["result"]

    source_field_id = call_method(
        "crm.deal.userfield.add",
        {
            "fields": {
                "FIELD_NAME": "UF_CRM_ENUM_ONE",
                "USER_TYPE_ID": "enumeration",
                "MULTIPLE": "N",
                "EDIT_FORM_LABEL": {"en": "Request source"},
                "LIST": [{"VALUE": "Website"}, {"VALUE": "Phone"}, {"VALUE": "Partner"}],
            },
        },
    )

    call_method(
        "crm.deal.userfield.add",
        {
            "fields": {
                "FIELD_NAME": "UF_CRM_ENUM_MULTI",
                "USER_TYPE_ID": "enumeration",
                "MULTIPLE": "Y",
                "EDIT_FORM_LABEL": {"en": "Client interests"},
                "LIST": [{"VALUE": "Training"}, {"VALUE": "Implementation"}, {"VALUE": "Support"}],
            },
        },
    )

    deal_fields = call_method("crm.deal.fields")

    source_id = int(
        next(
            item["ID"]
            for item in deal_fields["UF_CRM_ENUM_ONE"]["items"]
            if item["VALUE"] == "Website"
        )
    )
    interest_ids = [
        int(item["ID"])
        for item in deal_fields["UF_CRM_ENUM_MULTI"]["items"]
        if item["VALUE"] in ("Training", "Support")
    ]

    call_method(
        "crm.deal.update",
        {
            "id": DEAL_ID,
            "fields": {
                "UF_CRM_ENUM_ONE": source_id,
                "UF_CRM_ENUM_MULTI": interest_ids,
            },
        },
    )

    deal = call_method("crm.deal.get", {"id": DEAL_ID})

    deals_by_source = call_method(
        "crm.deal.list",
        {
            "filter": {"UF_CRM_ENUM_ONE": source_id},
            "select": ["ID", "TITLE", "UF_CRM_ENUM_ONE"],
        },
    )

    call_method(
        "crm.deal.userfield.update",
        {
            "id": source_field_id,
            "fields": {
                "LIST": [
                    {"ID": source_id, "VALUE": "Company Website"},
                    {"VALUE": "Advertising"},
                ],
            },
        },
    )

    after_rename = call_method("crm.deal.get", {"id": DEAL_ID})

    print(deal["UF_CRM_ENUM_ONE"], deal["UF_CRM_ENUM_MULTI"])
    print(deals_by_source)
    print(after_rename["UF_CRM_ENUM_ONE"])
    ```

{% endlist %}

## Continue Learning

- [{#T}](../../../api-reference/crm/universal/user-defined-fields/crm-userfield-types.md)
- [{#T}](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md)
- [{#T}](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-update.md)
- [{#T}](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md)
- [{#T}](../../../api-reference/crm/deals/crm-deal-fields.md)
- [{#T}](../../../api-reference/crm/deals/crm-deal-update.md)
- [{#T}](../../../api-reference/crm/deals/crm-deal-list.md)
