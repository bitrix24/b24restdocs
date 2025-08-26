# Add Basket Item Binding to Payment sale.paymentitembasket.add

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a binding of a basket item to a payment.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating a binding of a basket item to a payment ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **paymentId***
[`sale_order_payment.id`](../data-types.md) | Payment identifier ||
|| **basketId***
[`sale_basket_item.id`](../data-types.md) | Basket item identifier ||
|| **quantity**
[`double`](../../data-types.md) | Quantity of the product ||
|| **xmlId**
[`string`](../../data-types.md) | External record identifier ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"quantity":3,"basketId":2722,"paymentId":1025,"xmlId":"myXmlId"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paymentitembasket.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"quantity":3,"basketId":2722,"paymentId":1025,"xmlId":"myXmlId"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paymentitembasket.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.paymentitembasket.add', {
    			fields: {
    				quantity: 3,
    				basketId: 2722,
    				paymentId: 1025,
    				xmlId: 'myXmlId',
    			}
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
                'sale.paymentitembasket.add',
                [
                    'fields' => [
                        'quantity'  => 3,
                        'basketId'  => 2722,
                        'paymentId' => 1025,
                        'xmlId'     => 'myXmlId',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding payment item to basket: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.paymentitembasket.add', {
            fields: {
                quantity: 3,
                basketId: 2722,
                paymentId: 1025,
                xmlId: 'myXmlId',
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.paymentitembasket.add',
        [
            'fields' =>
            [
                'quantity' => 3,
                'basketId' => 2722,
                'paymentId' => 1025,
                'xmlId' => 'myXmlId',
            ]
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
    "result": {
        "paymentItemBasket": {
            "basketId": 2722,
            "dateInsert": "2024-04-17T09:37:45+02:00",
            "id": 1186,
            "paymentId": 1025,
            "quantity": 3,
            "xmlId": "myXmlId"
        }
    },
    "time": {
        "start": 1713339465.349855,
        "finish": 1713339465.968993,
        "duration": 0.6191380023956299,
        "processing": 0.38312196731567383,
        "date_start": "2024-04-17T10:37:45+02:00",
        "date_finish": "2024-04-17T10:37:45+02:00"
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
[`sale_payment_item_basket`](../data-types.md) | Object with information about the added binding of the basket item to the payment ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 201240400002,
    "error_description": "payment not exists"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201250000001` | An item with the specified field values `paymentId` and `basketId` already exists ||
|| `201240400002` | Payment not found. Incorrect value for the provided parameter `paymentId` ||
|| `201240400003` | Basket item not found. Incorrect value for the provided parameter `basketId` ||
|| `200040300020` | Insufficient permissions to add a binding of the basket item to the payment ||
|| `100` | The parameter `fields` is not specified or is empty ||
|| `0` | Required fields are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-payment-item-basket-update.md)
- [{#T}](./sale-payment-item-basket-get.md)
- [{#T}](./sale-payment-item-basket-list.md)
- [{#T}](./sale-payment-item-basket-delete.md)
- [{#T}](./sale-payment-item-basket-get-fields.md)