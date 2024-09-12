# Get a List of Payer Types sale.persontype.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of payer types.

Use the method `sale.persontype.list` when creating a payment system **for orders**. In CRM (for old invoices, deals), the list of payer types is obtained using the method [crm.persontype.list](../../crm/outdated/invoice/crm-person-type-list.md).

## Method Parameters

#|
|| **Parameter**
`type`| **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see fields of the object [sale_person_type](../data-types.md#sale_person_type))

If not provided or an empty array is passed, all available fields of payer types will be selected.
 ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected payer types in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [sale_person_type](../data-types.md#sale_person_type).

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `=` — equal (works with arrays as well)
- `%` — LIKE, substring search. The % symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string.
- `>` — greater than
- `<` — less than
- `!=` — not equal
- `!%` — NOT LIKE, substring search. The % symbol in the filter value does not need to be passed. The search goes from both sides.
- `>=` — greater than or equal to
- `<=` — less than or equal to
- `=%` — LIKE, substring search. The % symbol must be passed in the value. Examples: 
    - `"mol%"` — searching for values starting with "mol"
    - `"%mol"` — searching for values ending with "mol"
    - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!=%` — NOT LIKE, substring search. The % symbol must be passed in the value. Examples:
    - `"mol%"` — searching for values not starting with "mol"
    - `"%mol"` — searching for values not ending with "mol"
    - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (see description above)
 ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected payer types in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [sale_person_type](../data-types.md#sale_person_type)

Possible values for order:

- `asc` — in ascending order
- `desc` — in descending order
 ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for managing pagination.
 
The page size of results is always static: 50 records.
 
To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.
 
The formula for calculating the value of the `start` parameter:
 
`start = (N-1) * 50`, where `N` — the desired page number
 ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id", "name", "sort"],"filter":{"<=sort":100},"order":{"sort":"DESC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.persontype.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id", "name", "sort"],"filter":{"<=sort":100},"order":{"sort":"DESC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.persontype.list
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.persontype.list',
        {
            select: ["id", "name", "sort"],
            filter: {'<=sort': 100},
            order: {'sort': 'DESC'}
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.persontype.list',
        [
            'select' => ["id", "name", "sort"],
            'filter' => ['<=sort' => 100],
            'order' => ['sort' => 'DESC']
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
    "personTypes": [
        {	
            "id": 4,
            "name": "Legal Entity",
            "sort": "110"
        },
        {
            "id": 3,
            "name": "Individual",
            "sort": "100"
        },
        {
            "id": 12,
            "name": "Legal Entity",
            "sort": "100"
        }
    ],
    "total": 3,
    "time": {
        "start": 1712326352.63409,
        "finish": 1712326352.8319,
        "duration": 0.197818040847778,
        "processing": 0.00833678245544434,
        "date_start": "2024-04-05T16:12:32+02:00",
        "date_finish": "2024-04-05T16:12:32+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **personTypes**
[`sale_person_type[]`](../../data-types.md) | An array of objects containing information about the selected payer types ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 200040300010,
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | No access to read ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-person-type-add.md)
- [{#T}](./sale-person-type-update.md)
- [{#T}](./sale-person-type-get.md)
- [{#T}](./sale-person-type-delete.md)
- [{#T}](./sale-person-type-get-fields.md)