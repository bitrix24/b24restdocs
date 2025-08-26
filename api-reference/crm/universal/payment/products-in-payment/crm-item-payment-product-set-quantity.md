# Change Product Quantity crm.item.payment.product.setQuantity

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: access permission to modify the payment order is required

This method changes the quantity of a product in the payment line item.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../../data-types.md) | Identifier of the product line item in the payment ||
|| **quantity***
[`double`](../../../../data-types.md) | Quantity of the product ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1195,"quantity":3}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.payment.product.setQuantity
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1195,"quantity":3,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.payment.product.setQuantity
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.item.payment.product.setQuantity', {
    			id: 1195,
    			quantity: 3
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
                'crm.item.payment.product.setQuantity',
                [
                    'id'       => 1195,
                    'quantity' => 3,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting product quantity: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.payment.product.setQuantity', {
            id: 1195,
            quantity: 3
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
        'crm.item.payment.product.setQuantity',
        [
            'id' => 1195,
            'quantity' => 3
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
      "start":1716282831.283905,
      "finish":1716282832.02954,
      "duration":0.7456350326538086,
      "processing":0.4978060722351074,
      "date_start":"2024-05-21T12:13:51+03:00",
      "date_finish":"2024-05-21T12:13:52+03:00"
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
|| `0` | Product line item not found ||
|| `0` | Access denied ||
|| `0` | Quantity of the product cannot be less than or equal to 0 ||
|| `0` | Insufficient product to add to payment ||
|| `100` | Required fields not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-payment-product-add.md)
- [{#T}](./crm-item-payment-product-list.md)
- [{#T}](./crm-item-payment-product-delete.md)