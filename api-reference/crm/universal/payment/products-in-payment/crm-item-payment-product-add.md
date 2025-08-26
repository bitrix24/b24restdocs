# Add Product Item to Payment crm.item.payment.product.add

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: requires access permission to modify the order to which the product item is being added

This method adds a product item to the payment.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **paymentId***
[`sale_order_payment.id`](../../../../sale/data-types.md#sale_order_payment) | Payment identifier.
Can be obtained using the method [`sale.payment.list`](../../../../sale/payment/sale-payment-list.md)
 ||
 || **rowId***
[`integer`](../../../../data-types.md) | Identifier of the product item in the CRM object.
Can be obtained using [`crm.item.productrow.list`](../../../../crm/universal/product-rows/crm-item-productrow-list.md)
 ||
 || **quantity***
[`double`](../../../../data-types.md)| Quantity of the product ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"paymentId":1039,"rowId":17587,"quantity":2}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.payment.product.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"paymentId":1039,"rowId":17587,"quantity":2,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.payment.product.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.item.payment.product.add', {
    			paymentId: 1039,
    			rowId: 17587,
    			quantity: 2
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
                'crm.item.payment.product.add',
                [
                    'paymentId' => 1039,
                    'rowId'     => 17587,
                    'quantity'  => 2
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding payment product: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.payment.product.add', {
            paymentId: 1039,
            rowId: 17587,
            quantity: 2
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
        'crm.item.payment.product.add',
        [
            'paymentId' => 1039,
            'rowId' => 17587,
            'quantity' => 2
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
   "result":1193,
   "time":{
      "start":1716276648.349503,
      "finish":1716276649.261574,
      "duration":0.9120709896087646,
      "processing":0.6422691345214844,
      "date_start":"2024-05-21T10:30:48+02:00",
      "date_finish":"2024-05-21T10:30:49+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../../data-types.md) | Identifier of the product item in the payment ||
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
|| `0` | Product item not found ||
|| `0` | Insufficient product quantity to add to payment ||
|| `0` | Product quantity cannot be less than or equal to 0 ||
|| `100` | Required fields not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-payment-product-set-quantity.md)
- [{#T}](./crm-item-payment-product-list.md)
- [{#T}](./crm-item-payment-product-delete.md)