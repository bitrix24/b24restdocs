# Change Payment Binding to Shipment sale.paymentItemShipment.update

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method changes the binding of payment to shipment.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_payment_item_shipment.id`](../data-types.md) | Identifier of the payment binding to the shipment ||
|| **fields***
[`object`](../../data-types.md) | Field values (detailed description provided [below](#parametr-fields)) for creating the payment binding to the shipment in the form of a structure:

```js
fields: {
    xmlId: 'value'
}
```

||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
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
    -d '{"id":1181,"fields":{"xmlId":"myNewXmlId"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paymentitemshipment.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1181,"fields":{"xmlId":"myNewXmlId"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paymentitemshipment.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.paymentitemshipment.update', {
    			id: 1181,
    			fields: {
    				xmlId: 'myNewXmlId',
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
                'sale.paymentitemshipment.update',
                [
                    'id' => 1181,
                    'fields' => [
                        'xmlId' => 'myNewXmlId',
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
        echo 'Error updating payment item shipment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.paymentitemshipment.update', {
            id: 1181,
            fields: {
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.paymentitemshipment.update',
        [
            'id' => 1181,
            'fields' => [
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

HTTP status: **200**

```json
{
    "result": {
        "paymentItemShipment": {
            "dateInsert": "2024-04-15T09:22:26+02:00",
            "id": 1181,
            "paymentId": 1025,
            "shipmentId": 2471,
            "xmlId": "myNewXmlId"
        }
    },
    "time": {
        "start": 1713167192.849789,
        "finish": 1713167193.547071,
        "duration": 0.697282075881958,
        "processing": 0.4292449951171875,
        "date_start": "2024-04-15T10:46:32+02:00",
        "date_finish": "2024-04-15T10:46:33+02:00"
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
[`sale_payment_item_shipment`](../data-types.md) | Object with information about the updated payment binding to the shipment ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 201240400001,
    "error_description": "payment item does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201240400001` | The updated payment binding to the shipment was not found ||
|| `200040300020` | Insufficient rights to update the payment binding to the shipment ||
|| `100` | The parameter `id` is not specified ||
|| `100` | The parameter `fields` is not specified or is empty ||
|| `0` | Required fields of the `fields` structure are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-payment-item-shipment-add.md)
- [{#T}](./sale-payment-item-shipment-get.md)
- [{#T}](./sale-payment-item-shipment-list.md)
- [{#T}](./sale-payment-item-shipment-delete.md)
- [{#T}](./sale-payment-item-shipment-get-fields.md)