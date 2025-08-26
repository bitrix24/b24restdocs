# Create Notifications for Delivery Request sale.delivery.request.sendmessage

> Scope: [`sale, delivery`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method creates notifications for a delivery request.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **DELIVERY_ID***
[`sale_delivery_service.ID`](../../data-types.md) | Identifier of the delivery service related to the delivery request.

You can obtain the identifiers of `sale_delivery_service.ID` for delivery services using the [sale.delivery.getlist](../delivery/sale-delivery-get-list.md) method.
||
|| **REQUEST_ID***
[`string`](../../../data-types.md) | Identifier of the delivery request.

The identifier is assigned by the external system in response to the webhook for creating a delivery order (more details in the webhook description [Creating a Delivery Order](../webhooks/create-delivery-request.md)).
||
|| **ADDRESSEE***
[`string`](../../../data-types.md) | Recipient of the message.

Possible values:
- `MANAGER` — manager
- `RECIPIENT` — recipient of the cargo
||
|| **MESSAGE***
[`object`](../../../data-types.md) | Message (detailed description provided [below](#parametr-message))
||
|#

### MESSAGE Parameter {#parametr-message}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **SUBJECT***
[`string`](../../../data-types.md) | Subject of the message.

At least one of the following fields must be filled: `SUBJECT`, `BODY`
||
|| **BODY***
[`string`](../../../data-types.md) | Body of the message.

The body of the message may use macros to replace time and monetary values.

At least one of the following fields must be filled: `SUBJECT`, `BODY`
||
|| **STATUS**
[`object`](../../../data-types.md) | Status of the message (detailed description provided [below](#parametr-status))
||
|| **MONEY_VALUES**
[`object`](../../../data-types.md) | Object in the format `key => value`. 

Used to replace monetary values in the body of the message (see example below)
||
|#

### STATUS Parameter {#parametr-status}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **MESSAGE***
[`string`](../../../data-types.md) | Text name of the message status
||
|| **SEMANTIC***
[`string`](../../../data-types.md) | Value of the status semantics.

Possible values:
- `success` — success
- `process` — in process
- `error` — error
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DELIVERY_ID":225,"REQUEST_ID":"4757aca4931a4f029f49c0db4374d13d","ADDRESSEE":"MANAGER","MESSAGE":{"SUBJECT":"Your order is on its way","BODY":"Estimated delivery price: #MONEY#","MONEY_VALUES":{"#MONEY#":351.2},"STATUS":{"MESSAGE":"Success","SEMANTIC":"success"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.request.sendmessage
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DELIVERY_ID":225,"REQUEST_ID":"4757aca4931a4f029f49c0db4374d13d","ADDRESSEE":"MANAGER","MESSAGE":{"SUBJECT":"Your order is on its way","BODY":"Estimated delivery price: #MONEY#","MONEY_VALUES":{"#MONEY#":351.2},"STATUS":{"MESSAGE":"Success","SEMANTIC":"success"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.request.sendmessage
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.delivery.request.sendmessage', {
    			DELIVERY_ID: 225,
    			REQUEST_ID: "4757aca4931a4f029f49c0db4374d13d",
    			ADDRESSEE: "MANAGER",
    			MESSAGE: {
    				SUBJECT: "Your order is on its way",
    				BODY: "Estimated delivery price: #MONEY#",
    				MONEY_VALUES: {
    					"#MONEY#": 351.2,
    				},
    				STATUS: {
    					MESSAGE: "Success",
    					SEMANTIC: "success",
    				},
    			},
    		}
    	);
    	
    	const result = response.getData().result;
    	if (result.error()) {
    		console.error(result.error());
    	} else {
    		console.info(result);
    	}
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
                'sale.delivery.request.sendmessage',
                [
                    'DELIVERY_ID' => 225,
                    'REQUEST_ID' => "4757aca4931a4f029f49c0db4374d13d",
                    'ADDRESSEE' => "MANAGER",
                    'MESSAGE' => [
                        'SUBJECT' => "Your order is on its way",
                        'BODY' => "Estimated delivery price: #MONEY#",
                        'MONEY_VALUES' => [
                            "#MONEY#" => 351.2,
                        ],
                        'STATUS' => [
                            'MESSAGE' => "Success",
                            'SEMANTIC' => "success",
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error sending delivery message: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.delivery.request.sendmessage', {
            DELIVERY_ID: 225,
            REQUEST_ID: "4757aca4931a4f029f49c0db4374d13d",
            ADDRESSEE: "MANAGER",
            MESSAGE: {
                SUBJECT: "Your order is on its way",
                BODY: "Estimated delivery price: #MONEY#",
                MONEY_VALUES: {
                    "#MONEY#": 351.2,
                },
                STATUS: {
                    MESSAGE: "Success",
                    SEMANTIC: "success",
                },
            },
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
        'sale.delivery.request.sendmessage',
        [
            'DELIVERY_ID' => 225,
            'REQUEST_ID' => "4757aca4931a4f029f49c0db4374d13d",
            'ADDRESSEE' => "MANAGER",
            'MESSAGE' => [
                'SUBJECT' => "Your order is on its way",
                'BODY' => "Estimated delivery price: #MONEY#",
                'MONEY_VALUES' => [
                    "#MONEY#" => 351.2,
                ],
                'STATUS' => [
                    'MESSAGE' => "Success",
                    'SEMANTIC' => "success",
                ],
            ],
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
        "start":1714561617.200821,
        "finish":1714561617.526123,
        "duration":0.3253021240234375,
        "processing":0.1471860408782959,
        "date_start":"2024-05-01T14:06:57+02:00",
        "date_finish":"2024-05-01T14:06:57+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of sending the message for the delivery request ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error":"DELIVERY_NOT_FOUND",
    "error_description":"Delivery service has not been found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `DELIVERY_ID_NOT_SPECIFIED` | Delivery service identifier not specified | `400` || 
|| `DELIVERY_NOT_FOUND` | Delivery service not found | `400` || 
|| `REQUEST_ID_NOT_SPECIFIED` | Delivery request identifier not specified | `400` ||
|| `REQUEST_NOT_FOUND` | Delivery request not found | `400` ||
|| `ADDRESSEE_IS_NOT_SPECIFIED` | Message recipient not specified | `400` ||
|| `ADDRESSEE_UNEXPECTED_VALUE` | Unknown message recipient.

Allowed values:
- `MANAGER` — manager
- `RECIPIENT` — recipient of the cargo
| `400` ||
|| `MESSAGE_NOT_SPECIFIED` | Message not specified.

Either the subject or the body of the message must be specified
| `400` ||
|| `MESSAGE_STATUS_NOT_SPECIFIED` | Message status not specified
| `400` ||
|| `MESSAGE_STATUS_SEMANTIC_NOT_SPECIFIED` | Message status semantics not specified
| `400` ||
|| `UNEXPECTED_MESSAGE_STATUS_SEMANTIC` | Unknown message status semantics
| `400` ||
|| `REQUEST_SHIPMENT_NOT_FOUND` | Shipments linked to the specified delivery request not found
| `400` ||
|| `ACCESS_DENIED` | Insufficient rights to add the delivery service | `403` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-request-update.md)
- [{#T}](./sale-delivery-request-delete.md)