# Add Delivery Item to Payment crm.item.payment.delivery.add

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: requires access permission to modify the order to which the delivery item is added.

This method adds a delivery item to the payment.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **paymentId***
[`sale_order_payment.id`](../../../../sale/data-types.md#sale_order_payment) | Payment identifier.
Can be obtained using the method [`sale.payment.list`](../../../../sale/payment/sale-payment-list.md) ||
|| **deliveryId***
[`sale_order_shipment.id`](../../../../sale/data-types.md#sale_order_shipment) | Delivery identifier.
Can be obtained using the method [`crm.item.delivery.list`](../../delivery/crm-item-delivery-list.md) (key id) ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"paymentId":1039,"deliveryId":4072}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.payment.delivery.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"paymentId":1039,"deliveryId":4072,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.payment.delivery.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.item.payment.delivery.add', {
    			paymentId: 1039,
    			deliveryId: 4072
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.item.payment.delivery.add',
                [
                    'paymentId'  => 1039,
                    'deliveryId' => 4072,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding payment delivery: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.payment.delivery.add', {
            paymentId: 1039,
            deliveryId: 4072
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.payment.delivery.add',
        [
            'paymentId' => 1039,
            'deliveryId' => 4072
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
   "result":1199,
   "time":{
      "start":1716295802.83799,
      "finish":1716295804.17372,
      "duration":1.3357298374176025,
      "processing":0.8379831314086914,
      "date_start":"2024-05-21T15:50:02+02:00",
      "date_finish":"2024-05-21T15:50:04+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../../data-types.md) | Identifier of the delivery item in the payment ||
|| **time**
[`time`](../../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

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
|| `0` | Delivery item not found ||
|| `100` | Required parameters not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-payment-delivery-list.md)
- [{#T}](./crm-item-payment-delivery-delete.md)
- [{#T}](./crm-item-payment-delivery-set-delivery.md)