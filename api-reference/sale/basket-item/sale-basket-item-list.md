# Get the List of Items (Positions) in the Cart sale.basketitem.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: store manager

The method returns a set of cart items filtered by the specified criteria.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | 
An array of fields to be selected (see fields of the object [sale_basket_item](../data-types.md)).

If the array is not provided or is empty, all available fields from the product catalogs will be selected.
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected records in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [sale_basket_item](../data-types.md). 

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol must be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search is conducted from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol must be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is absent in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal ||
|| **order**
[`object`](../../data-types.md) | 
An object for sorting the selected records in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [sale_basket_item](../data-types.md).

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for pagination control.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number
||
|#

## Code Examples

{% include [Footnote on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","orderId","productId","name","price","currency"],"filter":{"@orderId":[5147,5146]},"order":{"id":"desc"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.basketitem.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","orderId","productId","name","price","currency"],"filter":{"@orderId":[5147,5146]},"order":{"id":"desc"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.basketitem.list
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.basketitem.list",
        {
            select: [
                'id',
                'orderId',
                'productId',
                'name',
                'price',
                'currency',
            ],
            filter: {
                '@orderId': [5147, 5146],
            },
            order: {
                id: 'desc',
            },
            start: 0,
        },
    )
        .then(
            function(result)
            {
                if (result.error())
                {
                    console.error(result.error());
                }
                else
                {
                    console.log(result.data);
                }
            },
            function(error)
            {
                console.info(error);
            }
        );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.basketitem.list',
        [
            'select' => [
                'id',
                'orderId',
                'productId',
                'name',
                'price',
                'currency',
            ],
            'filter' => [
                '@orderId' => [5147, 5146],
            ],
            'order' => [
                'id' => 'desc',
            ],
            'start' => 0,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "basketItems": [
            {
                "currency": "USD",
                "id": 6802,
                "name": "Office Chair",
                "orderId": 5147,
                "price": 1100,
                "productId": 4343
            },
            
            {
                "currency": "USD",
                "id": 6791,
                "name": "Catalog Chair",
                "orderId": 5146,
                "price": 900,
                "productId": 0
            },
            {
                "currency": "USD",
                "id": 6770,
                "name": "Assembly",
                "orderId": 5146,
                "price": 1110,
                "productId": 4342
            }
        ]
    },
    "total": 3,
    "time": {
        "start": 1713958546.058793,
        "finish": 1713958548.507179,
        "duration": 2.4483859539031982,
        "processing": 0.2580289840698242,
        "date_start": "2024-04-24T13:35:46+02:00",
        "date_finish": "2024-04-24T13:35:48+02:00",
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
|| **basketItems**
[`sale_basket_item[]`](../data-types.md) | An array of objects containing information about the selected cart items ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":0,
    "error_description":"error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-basket-item-add.md)
- [{#T}](./sale-basket-item-update.md)
- [{#T}](./sale-basket-item-get.md)
- [{#T}](./sale-basket-item-delete.md)
- [{#T}](./sale-basket-item-get-fields.md)
- [{#T}](./sale-basket-item-add-catalog-product.md)
- [{#T}](./sale-basket-item-update-catalog-product.md)
- [{#T}](./sale-basket-item-get-catalog-product-fields.md)