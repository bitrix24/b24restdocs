# Add Payment Binding to Shipment sale.paymentItemShipment.add

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a payment binding to a shipment.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values (detailed description provided [below](#parametr-fields)) for creating a payment binding to a shipment in the form of a structure:

```js
fields: {
    shipmentId: value,
    paymentId: value,
    xmlId: 'value'
}
```

||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **shipmentId***
[`sale_order_shipment.id`](../data-types.md) | Shipment identifier ||
|| **paymentId***
[`sale_order_payment.id`](../data-types.md) | Payment identifier ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier of the record ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"shipmentId":2471,"paymentId":1025,"xmlId":"myXmlId"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paymentitemshipment.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"shipmentId":2471,"paymentId":1025,"xmlId":"myXmlId"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paymentitemshipment.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.paymentitemshipment.add', {
    			fields: {
    				shipmentId: 2471,
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
                'sale.paymentitemshipment.add',
                [
                    'fields' => [
                        'shipmentId' => 2471,
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
        echo 'Error adding payment item shipment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.paymentitemshipment.add', {
            fields: {
                shipmentId: 2471,
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
        'sale.paymentitemshipment.add',
        [
            'fields' => [
                'shipmentId' => 2471,
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
        "paymentItemShipment": {
            "dateInsert": "2024-04-15T09:22:26+02:00",
            "id": 1181,
            "paymentId": 1025,
            "shipmentId": 2471,
            "xmlId": "myXmlId"
        }
    },
    "time": {
        "start": 1713165816.941795,
        "finish": 1713165817.220281,
        "duration": 0.2784857749938965,
        "processing": 0.045699119567871094,
        "date_start": "2024-04-15T10:23:36+02:00",
        "date_finish": "2024-04-15T10:23:37+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **paymentItemShipment**
[`sale_payment_item_shipment`](../data-types.md) | Object with information about the added payment binding to the shipment ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 201240400003,
    "error_description": "shipment not exists"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201250000001` | An element with the specified field values `shipmentId` and `paymentId` already exists.
 
If it is necessary to change the previously specified external identifier of the binding, use the method [sale.paymentitemshipment.update](./sale-payment-item-shipment-update.md)
||
|| `201240400002` | Payment not found. Incorrect value of the passed parameter `paymentId` ||
|| `201240400003` | Shipment not found. Incorrect value of the passed parameter `shipmentId` ||
|| `200040300020` | Insufficient rights to add payment binding to the shipment ||
|| `100` | Parameter `fields` is not specified or is empty ||
|| `0` | Required fields are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-payment-item-shipment-update.md)
- [{#T}](./sale-payment-item-shipment-get.md)
- [{#T}](./sale-payment-item-shipment-list.md)
- [{#T}](./sale-payment-item-shipment-delete.md)
- [{#T}](./sale-payment-item-shipment-get-fields.md)