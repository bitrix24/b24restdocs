# Get Values of All Fields for the Payment Item Basket Binding sale.paymentitembasket.get

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves the values of all fields for the payment item basket binding.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_payment_item_basket.id`](../data-types.md) | Identifier for the payment item basket binding ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1186}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paymentitembasket.get
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1186,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paymentitembasket.get
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.paymentitembasket.get', {
            id: 1186,
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
        'sale.paymentitembasket.get',
        [
            'id' => 1186
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
        "paymentItemBasket":{
            "basketId":2722,
            "dateInsert":"2024-04-17T10:51:03+03:00",
            "id":1186,
            "paymentId":1025,
            "quantity":3,
            "xmlId":"myXmlId"
        }
    },
    "time":{
        "start":1713343923.033388,
        "finish":1713343923.350584,
        "duration":0.3171961307525635,
        "processing":0.03599095344543457,
        "date_start":"2024-04-17T11:52:03+03:00",
        "date_finish":"2024-04-17T11:52:03+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **paymentItemBasket**
[`sale_payment_item_basket`](../data-types.md) | Information about the payment item basket binding ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":201240400001,
    "error_description":"payment item does not exist"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201240400001` | Payment item basket binding not found ||
|| `200040300010` | Insufficient permissions to read payment item basket binding data ||
|| `100` | Parameter `id` not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-payment-item-basket-add.md)
- [{#T}](./sale-payment-item-basket-update.md)
- [{#T}](./sale-payment-item-basket-list.md)
- [{#T}](./sale-payment-item-basket-delete.md)
- [{#T}](./sale-payment-item-basket-get-fields.md)