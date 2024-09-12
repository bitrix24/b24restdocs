# Reassign Delivery Item to Another Document crm.item.payment.delivery.setDelivery

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: access permission to modify the payment order is required

This method reassigns a delivery item to another delivery document.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../../data-types.md) | Identifier of the delivery item in the payment.
Can be obtained using the method [`crm.item.payment.delivery.list`](./crm-item-payment-delivery-list.md)  ||
|| **deliveryId***
[`sale_order_shipment.id`](../../../../sale/data-types.md#sale_order_shipment) | Identifier of the delivery.

Can be obtained using the method [`crm.item.delivery.list`](../../delivery/crm-item-delivery-list.md) (key id)  ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1201,"deliveryId":4073}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.payment.delivery.setDelivery
    ```

- cURL (OAuth) 

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1201,"deliveryId":4073,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.payment.delivery.setDelivery
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.item.payment.delivery.setDelivery', {
            id: 1201,
            deliveryId: 4073,
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
        'crm.item.payment.delivery.setDelivery',
        [
            'id' => 1201,
            'deliveryId' => 4073
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP status: **200**

```json
{
   "result":true,
   "time":{
      "start":1716296989.985067,
      "finish":1716296991.216063,
      "duration":1.2309961318969727,
      "processing":0.8141369819641113,
      "date_start":"2024-05-21T16:09:49+03:00",
      "date_finish":"2024-05-21T16:09:51+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../../data-types.md) | Result of the operation ||
|| **time**
[`time`](../../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":0,
   "error_description":"Payable item has not been found"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `0` | Delivery item not found ||
|| `0` | Access denied ||
|| `100` | Required fields not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-payment-delivery-add.md)
- [{#T}](./crm-item-payment-delivery-delete.md)
- [{#T}](./crm-item-payment-delivery-list.md)