# How to Work with the Binding to CRM Directories Field

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed rights is required — administrative access to the CRM section
>
> - [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) — a CRM administrator
> - [crm.status.entity.types](../../../api-reference/crm/status/crm-status-entity-types.md) — any user
> - [crm.status.entity.items](../../../api-reference/crm/status/crm-status-entity-items.md) — any user
> - [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) — a user with permission to modify items of a CRM object
> - [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) — a user with permission to read items of a CRM object

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The Binding to CRM Directories field stores the code of an option from a system CRM directory. The field settings specify the directory identifier, for example `INDUSTRY`, while the deal stores the string code of the selected option, `STATUS_ID`, for example `IT`.

Use deals as an example. Create the Client Industry field bound to the Industry directory, retrieve its options, save the selected code in the deal, and verify the result.

The scenario consists of four steps.

1. Create the field using the [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) method
2. Retrieve the directory options using the [crm.status.entity.items](../../../api-reference/crm/status/crm-status-entity-items.md) method
3. Write the selected `STATUS_ID` using the [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) method
4. Verify the value using the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method

As a result, the `UF_CRM_CLIENT_INDUSTRY` field stores the code `IT`. In the deal card, it corresponds to the Information Technologies value.

## Before You Start

Prepare the scenario data:

- **The deal where the field will be filled in.** You will need its `id`. For deals, `entityTypeId` equals `2`
- **Directory identifier.** In the example, it is `INDUSTRY`. The identifiers of the available directories are returned by the [crm.status.entity.types](../../../api-reference/crm/status/crm-status-entity-types.md) method
- **REST access.** A webhook or an application with the `crm` scope. Only a CRM administrator can create custom fields

Store the webhook URL in an environment variable and do not publish it in open code.

The examples use the deal with `id = 8417`. Replace this identifier with the identifier of your own deal.

For server-side JS examples with `B24Hook`, Node.js 18, 20, 22 or newer is required. For new projects, use version 22 or later. B24JsSDK is an ES module: save the code in an `.mjs` file or add `"type": "module"` to `package.json`.

For examples with b24pysdk, Python 3.9 or newer is required.

{% include [Example Note](../../../_includes/examples.md) %}

## 1. Create the Client Industry Field

The [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) method creates a custom field for all deals at once.

Pass in `fields`:

- `FIELD_NAME` — field code. The `UF_CRM_` prefix is added automatically if you pass the name without it
- `USER_TYPE_ID` — field type, `crm_status`
- `MULTIPLE` — the `N` value, because the deal holds a single client industry
- `EDIT_FORM_LABEL` — field title in the deal card, specified by language
- `SETTINGS.ENTITY_TYPE` — directory identifier, `INDUSTRY`

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const response = await $b24.actions.v2.call.make({
        method: 'crm.deal.userfield.add',
        params: {
            fields: {
                FIELD_NAME: 'CLIENT_INDUSTRY',
                USER_TYPE_ID: 'crm_status',
                MULTIPLE: 'N',
                EDIT_FORM_LABEL: { en: 'Client industry' },
                SETTINGS: { ENTITY_TYPE: 'INDUSTRY' }
            }
        },
        requestId: 'userfield-add-client-industry'
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    const fieldId = response.getData().result
    console.log(fieldId)
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

    $fieldId = $serviceBuilder
        ->core
        ->call('crm.deal.userfield.add', [
            'fields' => [
                'FIELD_NAME' => 'CLIENT_INDUSTRY',
                'USER_TYPE_ID' => 'crm_status',
                'MULTIPLE' => 'N',
                'EDIT_FORM_LABEL' => ['en' => 'Client industry'],
                'SETTINGS' => ['ENTITY_TYPE' => 'INDUSTRY'],
            ],
        ])
        ->getResponseData()
        ->getResult();

    print_r($fieldId);
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token=os.environ["B24_HOOK_TOKEN"],
    )
    # B24_HOOK_TOKEN = 'user_id/webhook_key'

    response = token.call_method("crm.deal.userfield.add", {
        "fields": {
            "FIELD_NAME": "CLIENT_INDUSTRY",
            "USER_TYPE_ID": "crm_status",
            "MULTIPLE": "N",
            "EDIT_FORM_LABEL": {"en": "Client industry"},
            "SETTINGS": {"ENTITY_TYPE": "INDUSTRY"},
        },
    })

    field_id = response["result"]
    print(field_id)
    ```

{% endlist %}

The method returns the identifier of the created field.

```json
{
    "result": 6007771
}
```

The full field name is `UF_CRM_CLIENT_INDUSTRY`. Further on, pass `useOriginalUfNames: "Y"` so that the universal CRM methods return and accept this name without converting it to camelCase.

Create the field once. When running the step again, use a different name or skip the creation if the field already exists.

## 2. Retrieve the Directory Options

The [crm.status.entity.items](../../../api-reference/crm/status/crm-status-entity-items.md) method returns the directory options. Pass the `INDUSTRY` value that was specified in the field settings to `entityId`.

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)

    const response = await $b24.actions.v2.call.make({
        method: 'crm.status.entity.items',
        params: { entityId: 'INDUSTRY' },
        requestId: 'status-entity-items-industry'
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    const items = response.getData().result
    console.table(items)
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

    $items = $serviceBuilder
        ->core
        ->call('crm.status.entity.items', ['entityId' => 'INDUSTRY'])
        ->getResponseData()
        ->getResult();

    print_r($items);
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token=os.environ["B24_HOOK_TOKEN"],
    )

    items = token.call_method(
        "crm.status.entity.items",
        {"entityId": "INDUSTRY"},
    )["result"]

    print(items)
    ```

{% endlist %}

Abbreviated response:

```json
{
    "result": [
        {
            "NAME": "Information Technologies",
            "SORT": 10,
            "STATUS_ID": "IT"
        },
        {
            "NAME": "Telecommunications",
            "SORT": 20,
            "STATUS_ID": "TELECOM"
        },
        {
            "NAME": "Manufacturing",
            "SORT": 30,
            "STATUS_ID": "MANUFACTURING"
        }
    ]
}
```

Show the `NAME` value to the user and, after the selection, save the corresponding `STATUS_ID`. For the Information Technologies option, it is the string `IT`.

{% note warning "" %}

Do not pass the numeric `ID` of a directory record, its name, or a code from another directory. The field expects the string `STATUS_ID` from the [crm.status.entity.items](../../../api-reference/crm/status/crm-status-entity-items.md) response for `INDUSTRY`.

The deal update method does not check that the value belongs to the directory and can store an incorrect value without an error. Validate the selected code on your side before writing.

{% endnote %}

## 3. Write the Selected STATUS_ID to the Deal

The [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) method updates the deal. Pass:

- `entityTypeId` — the `2` value for deals
- `id` — deal identifier
- `fields.UF_CRM_CLIENT_INDUSTRY` — the selected `STATUS_ID`, `IT` in the example
- `useOriginalUfNames` — the `Y` value, to use the original name of the custom field

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)

    const response = await $b24.actions.v2.call.make({
        method: 'crm.item.update',
        params: {
            entityTypeId: 2,
            id: 8417,
            useOriginalUfNames: 'Y',
            fields: {
                UF_CRM_CLIENT_INDUSTRY: 'IT'
            }
        },
        requestId: 'crm-item-update-industry'
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    console.log(response.getData().result.item.UF_CRM_CLIENT_INDUSTRY)
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

    $item = $serviceBuilder
        ->core
        ->call('crm.item.update', [
            'entityTypeId' => 2,
            'id' => 8417,
            'useOriginalUfNames' => 'Y',
            'fields' => [
                'UF_CRM_CLIENT_INDUSTRY' => 'IT',
            ],
        ])
        ->getResponseData()
        ->getResult()['item'];

    print_r($item['UF_CRM_CLIENT_INDUSTRY']);
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token=os.environ["B24_HOOK_TOKEN"],
    )

    item = token.call_method("crm.item.update", {
        "entityTypeId": 2,
        "id": 8417,
        "useOriginalUfNames": "Y",
        "fields": {
            "UF_CRM_CLIENT_INDUSTRY": "IT",
        },
    })["result"]["item"]

    print(item["UF_CRM_CLIENT_INDUSTRY"])
    ```

{% endlist %}

Abbreviated response:

```json
{
    "result": {
        "item": {
            "id": 8417,
            "title": "Client industry field check for the REST tutorial",
            "UF_CRM_CLIENT_INDUSTRY": "IT",
            "entityTypeId": 2
        }
    }
}
```

## 4. Verify the Stored Value

The [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method returns the deal. Pass the same `entityTypeId`, `id`, and `useOriginalUfNames`.

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)

    const response = await $b24.actions.v2.call.make({
        method: 'crm.item.get',
        params: {
            entityTypeId: 2,
            id: 8417,
            useOriginalUfNames: 'Y'
        },
        requestId: 'crm-item-get-industry'
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    const item = response.getData().result.item
    console.log(item.UF_CRM_CLIENT_INDUSTRY)
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

    $item = $serviceBuilder
        ->core
        ->call('crm.item.get', [
            'entityTypeId' => 2,
            'id' => 8417,
            'useOriginalUfNames' => 'Y',
        ])
        ->getResponseData()
        ->getResult()['item'];

    print_r($item['UF_CRM_CLIENT_INDUSTRY']);
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token=os.environ["B24_HOOK_TOKEN"],
    )

    item = token.call_method("crm.item.get", {
        "entityTypeId": 2,
        "id": 8417,
        "useOriginalUfNames": "Y",
    })["result"]["item"]

    print(item["UF_CRM_CLIENT_INDUSTRY"])
    ```

{% endlist %}

Abbreviated response:

```json
{
    "result": {
        "item": {
            "id": 8417,
            "title": "Client industry field check for the REST tutorial",
            "UF_CRM_CLIENT_INDUSTRY": "IT",
            "entityTypeId": 2
        }
    }
}
```

The method returns the code `IT` rather than the Information Technologies name. To show the name in your own interface, map the code to the `NAME` value from the [crm.status.entity.items](../../../api-reference/crm/status/crm-status-entity-items.md) response.

## Verify the Result

The scenario is successful if:

- the [crm.status.entity.items](../../../api-reference/crm/status/crm-status-entity-items.md) method returned an option with `STATUS_ID: "IT"` for the `INDUSTRY` directory
- after the deal update, the `UF_CRM_CLIENT_INDUSTRY` field contains the string `IT`
- a repeated [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) call returns the same value
- in the deal card, the Client Industry field shows the Information Technologies option

If the code in the response differs from the selected `STATUS_ID`, do not consider the update successful, even when [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) returned HTTP 200.

## Errors and Troubleshooting

If the method returns an error, check the request data.

#|
|| **Code or Error Text** | **Reason and Action** ||
|| Empty code with the text `The parameter entityId is not defined or invalid.` | `entityId` is not passed to [crm.status.entity.items](../../../api-reference/crm/status/crm-status-entity-items.md) or an empty value is passed. Specify `INDUSTRY` ||
|| Empty code with the text `The parameter entityId must be a string.` | An array or a value of another type is passed to `entityId`. Pass the directory identifier as a string ||
|| Empty code with the text `The 'FIELD_NAME' field is not found.` | The field name is not passed to [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md). Pass `FIELD_NAME` ||
|| `ERROR_CORE` with the text `Invalid custom field type` | An unknown type is passed to `USER_TYPE_ID`. For the binding to a CRM directory, specify `crm_status` ||
|| `100` with the text `Could not find value for parameter {id}` | The deal identifier is not passed to [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) ||
|| `NOT_FOUND` with the text `Item not found` | There is no deal with such an `id`, or the user has no permission to read it ||
|#

### The Method Returned Success but the Value Is Wrong

A `crm_status` field does not validate the value against the directory when written through REST. In a test Bitrix24, [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) returned HTTP 200 in all three cases, and [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) read the stored value:

- the numeric option `ID` `539` was stored as the string `"539"`
- the nonexistent code `NOT_A_REAL_INDUSTRY` was stored unchanged
- the `CALL` code from the `SOURCE` directory was stored in the field bound to `INDUSTRY`

The field setting behaves the same way. If the identifier of a nonexistent directory is passed to `SETTINGS.ENTITY_TYPE`, the field is created without an error and is bound to the first directory in the list. Such a field can be recognized by the [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) response: for a correctly configured field, `ENTITY_TYPE` comes as the string `INDUSTRY`, while for a substituted one it comes as an object with the `ID` and `NAME` fields of the directory that was substituted.

These are not method errors, so they are not included in the table above. Before the update, check that the selected value matches one of the `STATUS_ID` values retrieved for the required `entityId`. After the update, read the deal and compare the stored code with the one you sent.

## Key Points

- `INDUSTRY` is the directory identifier, and `IT` is the identifier of its option. These values cannot be swapped
- the field stores `STATUS_ID` rather than the numeric `ID` of the directory record or `NAME`
- [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) does not check whether the code exists and whether it belongs to the directory from `SETTINGS.ENTITY_TYPE`
- the directory options are retrieved before writing, and the selected code is checked against the current [crm.status.entity.items](../../../api-reference/crm/status/crm-status-entity-items.md) response
- with the `useOriginalUfNames: "Y"` parameter, the field is named `UF_CRM_CLIENT_INDUSTRY`, and without it the universal methods return it as `ufCrmClientIndustry`
- the field is created once, while retrieving the options and writing the value happen every time the user makes a selection

## Code Example

The code goes through all four steps: it creates the field, retrieves the `INDUSTRY` options, selects the Information Technologies option, writes its `STATUS_ID` to the deal, and verifies the result.

Replace the webhook and the deal identifier. If the field already exists, remove the [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) call from the example.

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    const dealId = 8417

    async function call(method, params, requestId) {
        const response = await $b24.actions.v2.call.make({ method, params, requestId })

        if (!response.isSuccess) {
            throw new Error(method + ': ' + response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }

    // 1. Create the field linked to the INDUSTRY directory
    await call('crm.deal.userfield.add', {
        fields: {
            FIELD_NAME: 'CLIENT_INDUSTRY',
            USER_TYPE_ID: 'crm_status',
            MULTIPLE: 'N',
            EDIT_FORM_LABEL: { en: 'Client industry' },
            SETTINGS: { ENTITY_TYPE: 'INDUSTRY' }
        }
    }, 'userfield-add-client-industry')

    // 2. Retrieve the options and take the STATUS_ID of the selected option
    const items = await call(
        'crm.status.entity.items',
        { entityId: 'INDUSTRY' },
        'status-entity-items-industry'
    )

    const selected = items.find((item) => item.NAME === 'Information Technologies')

    if (!selected) {
        throw new Error('The Information Technologies option is not found')
    }

    // 3. Write the selected STATUS_ID to the deal
    await call('crm.item.update', {
        entityTypeId: 2,
        id: dealId,
        useOriginalUfNames: 'Y',
        fields: {
            UF_CRM_CLIENT_INDUSTRY: selected.STATUS_ID
        }
    }, 'crm-item-update-industry')

    // 4. Read the deal and verify the stored code
    const item = (await call('crm.item.get', {
        entityTypeId: 2,
        id: dealId,
        useOriginalUfNames: 'Y'
    }, 'crm-item-get-industry')).item

    if (item.UF_CRM_CLIENT_INDUSTRY !== selected.STATUS_ID) {
        throw new Error('The stored value does not match the selected STATUS_ID')
    }

    console.log(selected.NAME, item.UF_CRM_CLIENT_INDUSTRY)
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

    $dealId = 8417;

    function callMethod($serviceBuilder, string $method, array $params = []): array
    {
        return $serviceBuilder
            ->core
            ->call($method, $params)
            ->getResponseData()
            ->getResult();
    }

    // 1. Create the field linked to the INDUSTRY directory
    callMethod($serviceBuilder, 'crm.deal.userfield.add', [
        'fields' => [
            'FIELD_NAME' => 'CLIENT_INDUSTRY',
            'USER_TYPE_ID' => 'crm_status',
            'MULTIPLE' => 'N',
            'EDIT_FORM_LABEL' => ['en' => 'Client industry'],
            'SETTINGS' => ['ENTITY_TYPE' => 'INDUSTRY'],
        ],
    ]);

    // 2. Retrieve the options and take the STATUS_ID of the selected option
    $items = callMethod(
        $serviceBuilder,
        'crm.status.entity.items',
        ['entityId' => 'INDUSTRY']
    );

    $selected = null;
    foreach ($items as $item) {
        if ($item['NAME'] === 'Information Technologies') {
            $selected = $item;
            break;
        }
    }

    if ($selected === null) {
        throw new RuntimeException('The Information Technologies option is not found');
    }

    // 3. Write the selected STATUS_ID to the deal
    callMethod($serviceBuilder, 'crm.item.update', [
        'entityTypeId' => 2,
        'id' => $dealId,
        'useOriginalUfNames' => 'Y',
        'fields' => [
            'UF_CRM_CLIENT_INDUSTRY' => $selected['STATUS_ID'],
        ],
    ]);

    // 4. Read the deal and verify the stored code
    $item = callMethod($serviceBuilder, 'crm.item.get', [
        'entityTypeId' => 2,
        'id' => $dealId,
        'useOriginalUfNames' => 'Y',
    ])['item'];

    if ($item['UF_CRM_CLIENT_INDUSTRY'] !== $selected['STATUS_ID']) {
        throw new RuntimeException('The stored value does not match the selected STATUS_ID');
    }

    echo $selected['NAME'] . ' ' . $item['UF_CRM_CLIENT_INDUSTRY'] . PHP_EOL;
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token=os.environ["B24_HOOK_TOKEN"],
    )

    deal_id = 8417

    # 1. Create the field linked to the INDUSTRY directory
    token.call_method("crm.deal.userfield.add", {
        "fields": {
            "FIELD_NAME": "CLIENT_INDUSTRY",
            "USER_TYPE_ID": "crm_status",
            "MULTIPLE": "N",
            "EDIT_FORM_LABEL": {"en": "Client industry"},
            "SETTINGS": {"ENTITY_TYPE": "INDUSTRY"},
        },
    })

    # 2. Retrieve the options and take the STATUS_ID of the selected option
    items = token.call_method(
        "crm.status.entity.items",
        {"entityId": "INDUSTRY"},
    )["result"]

    selected = next(
        (item for item in items if item["NAME"] == "Information Technologies"),
        None,
    )

    if selected is None:
        raise RuntimeError("The Information Technologies option is not found")

    # 3. Write the selected STATUS_ID to the deal
    token.call_method("crm.item.update", {
        "entityTypeId": 2,
        "id": deal_id,
        "useOriginalUfNames": "Y",
        "fields": {
            "UF_CRM_CLIENT_INDUSTRY": selected["STATUS_ID"],
        },
    })

    # 4. Read the deal and verify the stored code
    item = token.call_method("crm.item.get", {
        "entityTypeId": 2,
        "id": deal_id,
        "useOriginalUfNames": "Y",
    })["result"]["item"]

    if item["UF_CRM_CLIENT_INDUSTRY"] != selected["STATUS_ID"]:
        raise RuntimeError("The stored value does not match the selected STATUS_ID")

    print(selected["NAME"], item["UF_CRM_CLIENT_INDUSTRY"])
    ```

{% endlist %}

## Continue Learning

- [{#T}](../../../api-reference/crm/status/crm-status-entity-types.md)
- [{#T}](../../../api-reference/crm/status/crm-status-entity-items.md)
- [{#T}](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md)
- [{#T}](../../../api-reference/crm/universal/crm-item-update.md)
- [{#T}](../../../api-reference/crm/universal/crm-item-get.md)
