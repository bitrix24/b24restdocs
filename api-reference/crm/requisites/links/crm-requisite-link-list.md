# Get a List of Requisite Links crm.requisite.link.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a list of requisite links based on the filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | An array of fields to select (see [requisite link fields](#fields)).

If the array is not provided or an empty array is passed, all available requisite link fields will be selected. ||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected requisite links in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to [requisite link fields](#fields).

An additional prefix can be set for the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` character does not need to be passed in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` character needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` character does not need to be passed in the filter value. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` character needs to be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal 
||
|| **order**
[`object`](../../../data-types.md) | An object for sorting the selected requisite links in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to [requisite link fields](#fields).

Possible values for `order`:
- `asc` — ascending order
- `desc` — descending order
||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used for pagination control.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number
||
|#

### Description of Requisite Link Fields with CRM Object {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | The identifier of the object type to which the link belongs.

The following types can be used:
- deal (value `2`)
- old invoice (value `5`)
- estimate (value `7`)
- new invoice (value `31`)
- other dynamic objects (to get possible values, see the method [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md)).

CRM object type identifiers can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) 
||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) | The identifier of the object to which the link belongs. 

Object identifiers can be obtained using the following methods: [crm.deal.list](../../deals/crm-deal-list.md), [crm.quote.list](../../quote/crm-quote-list.md), [crm.item.list](../../universal/crm-item-list.md) ||
|| **REQUISITE_ID**
[`integer`](../../../data-types.md) | The identifier of the client's requisite selected for the object. 

Requisite identifiers can be obtained using the method [crm.requisite.list](../universal/crm-requisite-list.md) ||
|| **BANK_DETAIL_ID**
[`integer`](../../../data-types.md) | The identifier of the client's bank requisite selected for the object.

Bank requisite identifiers can be obtained using the method [crm.requisite.bankdetail.list](../bank-detail/crm-requisite-bank-detail-list.md) ||
|| **MC_REQUISITE_ID**
[`integer`](../../../data-types.md) | The identifier of my company's requisite selected for the object. 

Requisite identifiers can be obtained using the method [crm.requisite.list](../universal/crm-requisite-list.md) ||
|| **MC_BANK_DETAIL_ID**
[`integer`](../../../data-types.md) | The identifier of my company's bank requisite selected for the object. 

Bank requisite identifiers can be obtained using the method [crm.requisite.bankdetail.list](../bank-detail/crm-requisite-bank-detail-list.md) ||
|#

## Code Examples

{% include [Example Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ENTITY_ID":"ASC"},"filter":{"@ENTITY_TYPE_ID":[1,2,7,31]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.link.list
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"ENTITY_ID":"ASC"},"filter":{"@ENTITY_TYPE_ID":[1,2,7,31]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.link.list
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.requisite.link.list", {
            order: {"ENTITY_ID": "ASC"},
            filter: {"@ENTITY_TYPE_ID": [1, 2, 7, 31]}    // Leads, deals, estimates, invoices.
        },
        function (result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.dir(result.data());
                if (result.more())
                    result.next();
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.link.list',
        [
            'order' => ['ENTITY_ID' => 'ASC'],
            'filter' => ['@ENTITY_TYPE_ID' => [1, 2, 7, 31]]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "ENTITY_TYPE_ID": "7",
            "ENTITY_ID": "1",
            "REQUISITE_ID": "0",
            "BANK_DETAIL_ID": "0",
            "MC_REQUISITE_ID": "0",
            "MC_BANK_DETAIL_ID": "0"
        },
        {
            "ENTITY_TYPE_ID": "7",
            "ENTITY_ID": "2",
            "REQUISITE_ID": "0",
            "BANK_DETAIL_ID": "0",
            "MC_REQUISITE_ID": "0",
            "MC_BANK_DETAIL_ID": "0"
        },
        {
            "ENTITY_TYPE_ID": "7",
            "ENTITY_ID": "3",
            "REQUISITE_ID": "0",
            "BANK_DETAIL_ID": "0",
            "MC_REQUISITE_ID": "0",
            "MC_BANK_DETAIL_ID": "0"
        },
        {
            "ENTITY_TYPE_ID": "7",
            "ENTITY_ID": "4",
            "REQUISITE_ID": "7",
            "BANK_DETAIL_ID": "0",
            "MC_REQUISITE_ID": "2",
            "MC_BANK_DETAIL_ID": "2"
        },
        {
            "ENTITY_TYPE_ID": "7",
            "ENTITY_ID": "5",
            "REQUISITE_ID": "0",
            "BANK_DETAIL_ID": "0",
            "MC_REQUISITE_ID": "2",
            "MC_BANK_DETAIL_ID": "2"
        },
        {
            "ENTITY_TYPE_ID": "7",
            "ENTITY_ID": "6",
            "REQUISITE_ID": "0",
            "BANK_DETAIL_ID": "0",
            "MC_REQUISITE_ID": "2",
            "MC_BANK_DETAIL_ID": "2"
        },
        {
            "ENTITY_TYPE_ID": "2",
            "ENTITY_ID": "25",
            "REQUISITE_ID": "38",
            "BANK_DETAIL_ID": "0",
            "MC_REQUISITE_ID": "0",
            "MC_BANK_DETAIL_ID": "0"
        },
        {
            "ENTITY_TYPE_ID": "31",
            "ENTITY_ID": "315",
            "REQUISITE_ID": "60",
            "BANK_DETAIL_ID": "24",
            "MC_REQUISITE_ID": "2",
            "MC_BANK_DETAIL_ID": "2"
        }
    ],
    "total": 8,
    "time": {
        "start": 1718709631.410351,
        "finish": 1718709631.771324,
        "duration": 0.36097288131713867,
        "processing": 0.015230178833007812,
        "date_start": "2024-06-18T13:20:31+02:00",
        "date_finish": "2024-06-18T13:20:31+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md)| An array of objects with information from the selected requisite links. Each element contains the selected [requisite link fields](#fields) ||
|| **total**
[`integer`](../../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **40x**, **50x**

```json
{
    "error": 0,
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|  
|| **Code** | **Description** ||
|| `Access denied` | Insufficient access permissions to retrieve the list of requisite links ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-link-register.md)
- [{#T}](./crm-requisite-link-get.md)
- [{#T}](./crm-requisite-link-unregister.md)
- [{#T}](./crm-requisite-link-fields.md)