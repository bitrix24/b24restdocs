# Delete the binding of the cart item to the payment sale.paymentitembasket.delete

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method removes the binding of a cart item to a payment.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_payment_item_basket.id`](../data-types.md) | Identifier of the binding of the cart item to the payment
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1186}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paymentitembasket.delete
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1186,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paymentitembasket.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.paymentitembasket.delete', {
    			id: 1186,
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
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
                'sale.paymentitembasket.delete',
                [
                    'id' => 1186,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting payment item basket: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.paymentitembasket.delete', {
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.paymentitembasket.delete',
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

HTTP status: **200**

```json
{
    "result":true,
    "time":{
        "start":1713341821.787766,
        "finish":1713341822.437787,
        "duration":0.6500210762023926,
        "processing":0.40876007080078125,
        "date_start":"2024-04-17T11:17:01+02:00",
        "date_finish":"2024-04-17T11:17:02+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of removing the binding of the cart item to the payment ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

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
|| `201240400001` | Binding of the cart item to the payment not found ||
|| `200040300020` | Insufficient permissions to delete the binding of the cart item to the payment ||
|| `100` | Parameter `id` not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-payment-item-basket-add.md)
- [{#T}](./sale-payment-item-basket-update.md)
- [{#T}](./sale-payment-item-basket-get.md)
- [{#T}](./sale-payment-item-basket-list.md)
- [{#T}](./sale-payment-item-basket-get-fields.md)