# Import One Record crm.item.import

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with the "import" access permission for the CRM object.

A universal method for importing objects into CRM.

You can read about the differences between the import logic and the standard addition of elements in the article [{#T}](./index.md).

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type`          | **Description** ||
|| **entityTypeId***
[`integer`][3] | The identifier of the [system](../../data-types.md#object_type) or [custom type](../user-defined-object-types/index.md) for which you need to create an element.

Numerical values for system types (Lead — 1, Deal — 2, Contact — 3, Company — 4, Invoice — 31, etc.) are provided in the [CRM object types reference](../../data-types.md#object_type). The identifier of the SPA can be obtained using the [crm.type.list](../user-defined-object-types/crm-type-list.md) method. ||
|| **fields***
[`object`][3]  | An object in the following format:

```js
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

- `field_n` — the name of the field
- `value_n` — the value of the field

For multi-fields, such as `PHONE`, `EMAIL`, provide data in the [crm_multifield](../../data-types.md#crm_multifield) structure:

```js
{
    field_name: [
        {
            VALUE: "value_1",
            VALUE_TYPE: "type_1"
        },
        {
            VALUE: "value_2",
            VALUE_TYPE: "type_2"
        },
        ...
    ]
}
```

- `field_name` — the name of the field, for example, `PHONE`
- `VALUE` — the value of the field, for example, a phone number
- `VALUE_TYPE` — the type of value, for example, `WORK`

Each CRM object has its own set of fields. This means that the set of fields for creating a Lead does not have to match the set of fields for creating a Contact or SPA.

The list of available fields for each type of object is described [below](#parametr-fields).

An incorrect field in `fields` will be ignored.

You can also find out the set of fields using the universal method [crm.item.fields](../crm-item-fields.md) or methods for specific CRM objects:
- [crm.lead.fields](../../leads/crm-lead-fields.md)
- [crm.deal.fields](../../deals/crm-deal-fields.md)
- [crm.contact.fields](../../contacts/crm-contact-fields.md)
- [crm.company.fields](../../companies/crm-company-fields.md)
- [crm.quote.fields](../../quote/crm-quote-fields.md)
||
|| **useOriginalUfNames**
[`boolean`][1] | A parameter to control the format of custom field names in the request and response.   
Possible values:

- `Y` — original names of custom fields, for example, `UF_CRM_2_1639669411830`
- `N` — names of custom fields in camelCase, for example, `ufCrm2_1639669411830`

Default is `N` ||
|#

{% include [Parameter fields in different entities](../../_include/crm-entity-fields-list.md) %}

To upload a file, the value of the custom field must be an array where the first element is the file name and the second is the base64 encoded content of the file.

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

1. How to Import a Deal

   {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"fields":{"title":"New Deal (specifically for REST method example)","typeId":"SERVICE","categoryId":9,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be additional description about the source","leadId":102,"additionalInfo":"There should be additional information","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":2}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.import
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"entityTypeId":2,"fields":{"title":"New Deal (specifically for REST method example)","typeId":"SERVICE","categoryId":9,"stageId":"C9:UC_KN8KFI","isReccurring":"Y","probability":50,"currencyId":"USD","isManualOpportunity":"Y","opportunity":999.99,"taxValue":99.9,"companyId":5,"contactId":4,"contactIds":[4,5],"quoteId":7,"begindate":"formatDate(monthAgo)","closedate":"formatDate(twelveDaysInAdvance)","opened":"N","comments":"commentsExample","assignedById":6,"sourceId":"WEB","sourceDescription":"There should be additional description about the source","leadId":102,"additionalInfo":"There should be additional information","observers":[2,3],"utmSource":"google","utmMedium":"CPC","ufCrm_1721244707107":1111.1,"parentId1220":2},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.item.import
        ```

    - BX24.js

        ```js
        const formatDate = (date) => {
            return date.toISOString().slice(0, 10);
        };

        const day = 60 * 60 * 24 * 1000;

        const now = new Date();
        const twelveDaysInAdvance = new Date(now.getTime() + 12 * day);
        const monthAgo = new Date(now.getTime() - 30 * day);

        const commentsExample = `
        Example comment within the deal

        [B]Bold text[/B]
        [I]Italic[/I]
        [U]Underlined[/U]
        [S]Strikethrough[/S]
        [B][I][U][S]Mix[/S][/U][/I][/B]

        [LIST]
        [*]List item #1
        [*]List item #2
        [*]List item #3
        [/LIST]

        [LIST=1]
        [*]Numbered list item #1
        [*]Numbered list item #2
        [*]Numbered list item #3
        [/LIST]
        `;

        BX24.callMethod(
            'crm.item.import', 
            {
                entityTypeId: 2,
                fields: 
                {
                    title: "New Deal (specifically for REST method example)",
                    typeId: "SERVICE",
                    categoryId: 9,
                    stageId: "C9:UC_KN8KFI",
                    isReccurring: "Y",
                    probability: 50,
                    currencyId: "USD",
                    isManualOpportunity: "Y",
                    opportunity: 999.99,
                    taxValue: 99.9,
                    companyId: 5,
                    contactId: 4,
                    contactIds: [4, 5],
                    quoteId: 7,
                    begindate: formatDate(monthAgo),
                    closedate: formatDate(twelveDaysInAdvance),
                    opened: "N",
                    comments: commentsExample,
                    assignedById: 6,
                    sourceId: "WEB",
                    sourceDescription: "There should be additional description about the source",
                    leadId: 102,
                    additionalInfo: "There should be additional information",
                    observers: [2, 3],
                    utmSource: "google",
                    utmMedium: "CPC",
                    ufCrm_1721244707107: 1111.1,
                    parentId1220: 2,
                },
            },
            (result) => 
            {
                result.error() 
                    ? console.error(result.error()) 
                    : console.info(result.data())
                ;
            }
        );
        ```

    - PHP CRest

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.item.import',
            [
                'entityTypeId' => 2,
                'fields' => [
                    'title' => "New Deal (specifically for REST method example)",
                    'typeId' => "SERVICE",
                    'categoryId' => 9,
                    'stageId' => "C9:UC_KN8KFI",
                    'isReccurring' => "Y",
                    'probability' => 50,
                    'currencyId' => "USD",
                    'isManualOpportunity' => "Y",
                    'opportunity' => 999.99,
                    'taxValue' => 99.9,
                    'companyId' => 5,
                    'contactId' => 4,
                    'contactIds' => [4, 5],
                    'quoteId' => 7,
                    'begindate' => formatDate(monthAgo),
                    'closedate' => formatDate(twelveDaysInAdvance),
                    'opened' => "N",
                    'comments' => $commentsExample,
                    'assignedById' => 6,
                    'sourceId' => "WEB",
                    'sourceDescription' => "There should be additional description about the source",
                    'leadId' => 102,
                    'additionalInfo' => "There should be additional information",
                    'observers' => [2, 3],
                    'utmSource' => "google",
                    'utmMedium' => "CPC",
                    'ufCrm_1721244707107' => 1111.1,
                    'parentId1220' => 2,
                ],
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

   {% endlist %}


2. How to Create an SPA Element with a Set of Custom Fields

    {% cut "Custom Fields Used in the Example" %}

    {% include [Set of Custom Fields](../../_include/user-fields-for-examples-cut.md) %}

    {% endcut %}

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{
            "entityTypeId": 1302,
            "fields": {
                "ufCrm44_1721812760630": "String for custom field of type String",
                "ufCrm44_1721812814433": 81,
                "ufCrm44_1721812853419": "'"$(date '+%Y-%m-%d')"'",
                "ufCrm44_1721812885588": [
                    "example.com",
                    "second-example.com"
                ],
                "ufCrm44_1721812898903": [
                    "green_pixel.png",
                    "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="
                ],
                "ufCrm44_1721812915476": "300|USD",
                "ufCrm44_1721812935209": "Y",
                "ufCrm44_1721812948498": 9999.9
            }
        }' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.import
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{
            "entityTypeId": 1302,
            "fields": {
                "ufCrm44_1721812760630": "String for custom field of type String",
                "ufCrm44_1721812814433": 81,
                "ufCrm44_1721812853419": "'"$(date '+%Y-%m-%d')"'",
                "ufCrm44_1721812885588": [
                    "example.com",
                    "second-example.com"
                ],
                "ufCrm44_1721812898903": [
                    "green_pixel.png",
                    "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg=="
                ],
                "ufCrm44_1721812915476": "300|USD",
                "ufCrm44_1721812935209": "Y",
                "ufCrm44_1721812948498": 9999.9
            },
            "auth": "**put_access_token_here**"
        }' \
        https://**put_your_bitrix24_address**/rest/crm.item.import
        ```

    - BX24.js

        ```js
        const greenPixelInBase64 = "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg==";

        BX24.callMethod(
            'crm.item.import', 
            {
                entityTypeId: 1302,
                fields: {
                    ufCrm44_1721812760630: "String for custom field of type String",
                    ufCrm44_1721812814433: 81,
                    ufCrm44_1721812853419: (new Date()).toISOString().slice(0, 10),
                    ufCrm44_1721812885588: [
                        "example.com",
                        "second-example.com",
                    ],
                    ufCrm44_1721812898903: [
                        "green_pixel.png",
                        greenPixelInBase64,
                    ],
                    ufCrm44_1721812915476: "300|USD",
                    ufCrm44_1721812935209: "Y",
                    ufCrm44_1721812948498: 9999.9,
                },
            },
            (result) => 
            {
                result.error() 
                    ? console.error(result.error()) 
                    : console.info(result.data())
                ;
            }
        );
        ```

    - PHP CRest

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.item.import',
            [
                'entityTypeId' => 1302,
                'fields' => [
                    'ufCrm44_1721812760630' => "String for custom field of type String",
                    'ufCrm44_1721812814433' => 81,
                    'ufCrm44_1721812853419' => date('Y-m-d'),
                    'ufCrm44_1721812885588' => [
                        "example.com",
                        "second-example.com",
                    ],
                    'ufCrm44_1721812898903' => [
                        "green_pixel.png",
                        "iVBORw0KGgoAAAANSUhEUgAAAIAAAAAMCAYAAACqTLVoAAAALklEQVR42u3SAQEAAAQDsEsuOj3YMqwy6fBWCSCAAAIgAAIgAAIgAAIgAAJw3QLOrRH1U/gU4gAAAABJRU5ErkJggg==",
                    ],
                    'ufCrm44_1721812915476' => "300|USD",
                    'ufCrm44_1721812935209' => "Y",
                    'ufCrm44_1721812948498' => 9999.9,
                ],
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

   {% endlist %}

## Response Handling

The method will return an `item` array with the identifier of the created element in case of success, or an error message.

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 4
        }
    },
    "time": {
        "start": 1722940215.145257,
        "finish": 1722940217.94124,
        "duration": 2.795983076095581,
        "processing": 2.4315829277038574,
        "date_start": "2024-08-06T10:30:15+00:00",
        "date_finish": "2024-08-06T10:30:17+00:00",
        "operating": 2.4314892292022705
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][3] | The root element of the response. 

Contains a single key — `item` ||
|| **item**
[`object`][3] | Information about the created element. 

Contains a single key — `id` ||
|| **id**
[`int`][3] | The identifier of the created element ||
|| **time**
[`time`][3] | Information about the execution time of the request ||
|#

{% note info " " %}

By default, custom field names are passed and returned in camelCase, for example, `ufCrm2_1639669411830`.
When passing the `useOriginalUfNames` parameter with the value `Y`, custom fields will be returned with their original names, for example, `UF_CRM_2_1639669411830`.

{% endnote %}

## Error Handling

HTTP status: **401**, **400**, **403**

```json
{
    "error": "NOT_FOUND",
    "error_description": "SPA not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code**                           | **Description**                                                       | **Value**                                                                                    ||
|| `400`      | `NOT_FOUND`                       | SPA not found                                                      | Occurs when an invalid `entityTypeId` is passed                                              ||
|| `400`      | `ACCESS_DENIED`                   | Access denied                                                    | The user does not have permission to add elements of type `entityTypeId`                             ||
|| `400`      | `CRM_FIELD_ERROR_VALUE_NOT_VALID` | Invalid value for field "`field`"                                   | An incorrect value for the field `field` was provided.

For system fields of type `createdTime`, if the request is not made by an administrator ||
|| `400`      | `100`                             | Expected iterable value for multiple field, but got `type` instead | A value of type `type` was passed to one of the multiple fields, while an iterable type was expected. This can also occur with an incorrect request (invalid JSON or request headers) ||
|| `400`      | `CREATE_DYNAMIC_ITEM_RESTRICTED`  | You cannot create a new item due to your plan's restrictions | Plan restrictions do not allow the creation of SPA elements                              ||
|| `401`      | `INVALID_CREDENTIALS`             | Invalid authorization data for the request                            | Incorrect `user ID` and/or code in the request path                                       ||
|| `403`      | `allowed_only_intranet_user`      | Action allowed only for intranet users                   | The user is not an intranet user                                                 ||
|#

{% include [system errors](./../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./crm-item-batch-import.md)

[3]: ../../data-types.md