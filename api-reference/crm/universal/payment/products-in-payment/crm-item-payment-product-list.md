# Get a List of Payment Product Items crm.item.payment.product.list

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: read access permission for the payment order is required

This method retrieves a list of product items (goods or services) associated with a specific payment.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **paymentId***
[`sale_order_payment.id`](../../../../sale/data-types.md#sale_order_payment) | Identifier of the payment.
Can be obtained using the [`sale.payment.list`](../../../../sale/payment/sale-payment-list.md) method ||
|| **filter***
[`object`](../../../../data-types.md) | Object for filtering selected payment product items in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.
 
Possible values for `field`:
- `id`
- `quantity`

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:

- `=` — equals, exact match (used by default)
- `%` — LIKE, substring search. The % symbol should not be included in the filter value. The search looks for the substring in any position of the string.
- `>` — greater than
- `<` — less than
- `!=` — not equal
- `!%` — NOT LIKE, substring search. The % symbol should not be included in the filter value. The search goes from both sides.
- `>=` — greater than or equal to
- `<=` — less than or equal to
- `=%` — LIKE, substring search. The % symbol should be included in the value. Examples: 
    - `"mol%"` — searching for values starting with "mol"
    - `"%mol"` — searching for values ending with "mol"
    - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!=%` — NOT LIKE, substring search. The % symbol should be included in the value. Examples:
    - `"mol%"` — searching for values not starting with "mol"
    - `"%mol"` — searching for values not ending with "mol"
    - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (see description above)
||
|| **order**
[`object`](../../../../data-types.md) | Object for sorting selected payment product items in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.
 
Possible values for `field`:
- `id`
- `quantity`
 
Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
 ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"paymentId":1039,"filter":{"\u003E=quantity":2,"@id":[1195,1196]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.payment.product.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"paymentId":1039,"filter":{"\u003E=quantity":2,"@id":[1195,1196]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.payment.product.list
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.item.payment.product.list', {
            paymentId: 1039,
            filter: {
                ">=quantity": 2,
                "@id": [1195, 1196],
            },
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.payment.product.list',
        [
            'paymentId' => 1039,
            'filter' => [
                ">=quantity" => 2,
                "@id" => [1195, 1196],
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP Status: **200**

```json
{
   "result":[
      {
         "id":1195,
         "paymentId":1039,
         "quantity":2,
         "rowId":17587
      },
      {
         "id":1196,
         "paymentId":1039,
         "quantity":3,
         "rowId":17588
      }
   ],
   "time":{
      "start":1716286140.489916,
      "finish":1716286140.802505,
      "duration":0.3125889301300049,
      "processing":0.053195953369140625,
      "date_start":"2024-05-21T13:09:00+03:00",
      "date_finish":"2024-05-21T13:09:00+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
`crm_item_payment_product[]` | Array of objects containing information about the selected payment product items ||
|| **time**
[`time`](../../../../data-types.md) | Information about the execution time of the request ||
|#

### Key result. Object of type crm_item_payment_product 

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the product item in the payment ||
|| **paymentId**
[`sale_order_payment.id`](../../../../sale/data-types.md#sale_order_payment) | Identifier of the payment ||
|| **quantity**
[`double`](../../../../data-types.md) | Quantity of the product ||
|| **rowId**
[`integer`](../../../../data-types.md) | Identifier of the product item in the CRM entity ||
|#

## Error Handling

HTTP Status: **400**

```json
{
   "error":0,
   "error_description":"Payment has not been found"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `0` | Payment not found ||
|| `0` | Access denied ||
|| `100` | Required fields not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-payment-product-add.md)
- [{#T}](./crm-item-payment-product-set-quantity.md)
- [{#T}](./crm-item-payment-product-delete.md)