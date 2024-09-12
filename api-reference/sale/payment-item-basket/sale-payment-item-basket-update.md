# Update the Binding of the Cart Item to Payment sale.paymentitembasket.update

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the binding of a cart item to a payment.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_payment_item_basket.id`](../data-types.md) | Identifier of the binding of the cart item to the payment ||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the binding of the cart item to the payment ||
|#

### Parameter fields

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **quantity***
[`double`](../../data-types.md) | Quantity of the product ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier of the record ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1186,"fields":{"quantity":1,"xmlId":"myNewXmlId"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paymentitembasket.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1186,"fields":{"quantity":1,"xmlId":"myNewXmlId"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paymentitembasket.update
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.paymentitembasket.update', {
            id: 1186,
            fields: {
                quantity: 1,
                xmlId: 'myNewXmlId',
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
        'sale.paymentitembasket.update',
        [
            'id' => 1186,
            'fields' =>
            [
                'quantity' => 1,
                'xmlId' => 'myNewXmlId',
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
        "paymentItemBasket":{
            "basketId":2722,
            "dateInsert":"2024-04-17T09:37:45+03:00",
            "id":1186,
            "paymentId":1025,
            "quantity":1,
            "xmlId":"myNewXmlId"
        }
    },
    "time":{
        "start":1713341342.331169,
        "finish":1713341343.013559,
        "duration":0.6823902130126953,
        "processing":0.4167962074279785,
        "date_start":"2024-04-17T11:09:02+03:00",
        "date_finish":"2024-04-17T11:09:03+03:00"
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
[`sale_payment_item_basket`](../data-types.md) | Object containing information about the updated binding of the cart item to the payment ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":201240400001,
    "error_description":"payment item does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201240400001` | The updated binding of the cart item to the payment was not found ||
|| `200040300020` | Insufficient permissions to update the binding of the cart item to the payment ||
|| `100` | The `id` parameter is missing ||
|| `100` | The `fields` parameter is missing or empty ||
|| `0` | Required fields in the `fields` structure are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-payment-item-basket-add.md)
- [{#T}](./sale-payment-item-basket-get.md)
- [{#T}](./sale-payment-item-basket-list.md)
- [{#T}](./sale-payment-item-basket-delete.md)
- [{#T}](./sale-payment-item-basket-get-fields.md)