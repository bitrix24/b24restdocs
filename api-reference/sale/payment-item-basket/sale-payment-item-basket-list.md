# Get a List of Basket Item Bindings to Payments sale.paymentitembasket.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of bindings of basket items to payments.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of fields to select (see fields of the [sale_payment_item_basket](../data-types.md) object).

If the array is not provided or an empty array is passed, all available fields of the bindings of basket items to payments will be selected.
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected bindings of basket items to payments in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [sale_payment_item_basket](../data-types.md) object.

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected bindings of basket items to payments in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [sale_payment_item_basket](../data-types.md) object.

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

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","paymentId","basketId","quantity","xmlId","dateInsert"],"filter":{"paymentId":1025,">quantity":2},"order":{"quantity":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paymentitembasket.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","paymentId","basketId","quantity","xmlId","dateInsert"],"filter":{"paymentId":1025,">quantity":2},"order":{"quantity":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paymentitembasket.list
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.paymentitembasket.list', {
            select: [
                "id",
                "paymentId",
                "basketId",
                "quantity",
                "xmlId",
                "dateInsert",
            ],
            filter: {
                "paymentId": 1025,
                ">quantity": 2,
            },
            "order": {
                "quantity": "desc",
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.paymentitembasket.list',
        [
            'select' => [
                "id",
                "paymentId",
                "basketId",
                "quantity",
                "xmlId",
                "dateInsert",
            ],
            'filter' => [
                "paymentId" => 1025,
                ">quantity" => 2,
            ],
            'order' => [
                "quantity" => "desc",
            ]
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
    "result":{
        "paymentItemsBasket":[
            {
                "basketId":2722,
                "dateInsert":"2024-04-17T10:51:03+03:00",
                "id":1186,
                "paymentId":1025,
                "quantity":3,
                "xmlId":"myXmlId"
            }
        ]
    },
    "total":1,
    "time":{
        "start":1713344692.788531,
        "finish":1713344693.055679,
        "duration":0.2671480178833008,
        "processing":0.017076969146728516,
        "date_start":"2024-04-17T12:04:52+03:00",
        "date_finish":"2024-04-17T12:04:53+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **paymentItemBasket**
[`sale_payment_item_basket[]`](../data-types.md) | An array of objects containing information about the selected bindings of basket items to payments ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

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
|| `200040300010` | Insufficient permissions to read bindings of basket items to payments ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-payment-item-basket-add.md)
- [{#T}](./sale-payment-item-basket-update.md)
- [{#T}](./sale-payment-item-basket-get.md)
- [{#T}](./sale-payment-item-basket-delete.md)
- [{#T}](./sale-payment-item-basket-get-fields.md)