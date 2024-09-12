# Remove Delivery Item from Payment crm.item.payment.delivery.delete

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: requires permission to modify the order from which the delivery item is removed

This method removes a delivery item from the payment.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../../data-types.md) | Identifier of the delivery item in the payment.
Can be obtained using the method [`crm.item.payment.delivery.list`](./crm-item-payment-delivery-list.md)  ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1199}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.payment.delivery.delete
    ```

- cURL (OAuth) 

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1199,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.payment.delivery.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.item.payment.delivery.delete', {
            id: 1199,
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
        'crm.item.payment.delivery.delete',
        [
            'id' => 1199
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
   "result":true,
   "time":{
      "start":1716296091.028902,
      "finish":1716296092.17101,
      "duration":1.1421079635620117,
      "processing":0.7571370601654053,
      "date_start":"2024-05-21T15:54:51+03:00",
      "date_finish":"2024-05-21T15:54:52+03:00"
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
[`time`](../../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

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
|| `100` | Parameter id not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-payment-delivery-add.md)
- [{#T}](./crm-item-payment-delivery-list.md)
- [{#T}](./crm-item-payment-delivery-set-delivery.md)