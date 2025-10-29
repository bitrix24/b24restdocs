# Add Delivery Service Handler sale.delivery.handler.add

> Scope: [`sale`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

This method adds a delivery service handler.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
[`string`](../../../data-types.md) | Name of the delivery service handler ||
|| **CODE***
[`string`](../../../data-types.md) | Symbolic code of the delivery service handler ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting ||
|| **DESCRIPTION**
[`integer`](../../../data-types.md) | Description of the delivery service handler ||
|| **SETTINGS***
[`object`](../../../data-types.md) | Object containing information about the settings of the delivery service handler (detailed description provided [below](#parametr-settings)) ||
|| **PROFILES***
[`object[]`](../../../data-types.md) | Array containing a list of delivery profile objects (detailed description provided [below](#parametr-profiles)).

It is implied that the delivery service handler must have at least 1 profile ||
|#

### SETTINGS Parameter

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CALCULATE_URL***
[`string`](../../../data-types.md) | URL for calculating delivery cost.

Data about the shipment (what to deliver, where, and how) is sent to this URL, for which the delivery cost needs to be calculated in the response.

The request and response format is detailed in the documentation for the webhook **Calculate Delivery Cost** ||
|| **CREATE_DELIVERY_REQUEST_URL**
[`string`](../../../data-types.md) | URL for creating a delivery order.

Data about the shipment (what to deliver, where, and how) is sent to this URL, for which an order needs to be placed with the delivery service.

The request and response format is detailed in the documentation for the webhook **Create Delivery Order** ||
|| **CANCEL_DELIVERY_REQUEST_URL**
[`string`](../../../data-types.md) | URL for canceling a delivery order.

Data about the shipment (what to deliver, where, and how) is sent to this URL, for which the order needs to be canceled with the delivery service.

The request and response format is detailed in the documentation for the webhook **Cancel Delivery Order** ||
|| **HAS_CALLBACK_TRACKING_SUPPORT**
[`string`](../../../data-types.md) | Indicator of whether the delivery service will send notifications about the status of the delivery order (see method [`sale.delivery.request.sendmessage`](../delivery-request/sale-delivery-request-send-message.md)). 

If event support is indicated, a delivery activity will be created in the manager's interface when ordering delivery, into which changes related to the current delivery status can be transmitted.

Possible values:
- `Y` — support available
- `N` — no support
 ||
 || **CONFIG**
[`object[]`](../../../data-types.md) | Array of objects with available settings for the delivery service created using this handler (detailed description provided [below](#parametr-config)) ||
|#

### CONFIG Parameter

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TYPE***
[`string`](../../../data-types.md) | Type of the setting field
Possible values:
- `STRING` — string
- `Y/N` — checkbox (yes / no)
- `NUMBER` — number
- `ENUM` — list
- `DATE` — date
- `LOCATION` — location
 ||
|| **CODE***
[`string`](../../../data-types.md) | Symbolic code of the setting ||
|| **NAME***
[`string`](../../../data-types.md) | Name of the setting  ||
|| **OPTIONS**
[`object`](../../../data-types.md) | List of options for selection. Object in the format `key=value`. Where key is the option code, and value is the option.

Example:
```
{
   "Option1Code": "Option1Value",
   "Option2Code": "Option2Value",
   "Option3Code": "Option3Value"
}
```

This parameter is relevant only for settings of type `ENUM`
 ||
|#

### PROFILES Parameter

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
[`string`](../../../data-types.md) | Name of the delivery service handler profile  ||
|| **CODE***
[`string`](../../../data-types.md) | Symbolic code of the delivery service handler profile ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Description of the delivery service handler profile  ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"uber","NAME":"Uber","DESCRIPTION":"Uber Description","SORT":250,"SETTINGS":{"CALCULATE_URL":"http://gateway.bx/calculate.php","CREATE_DELIVERY_REQUEST_URL":"http://gateway.bx/create_delivery_request.php","CANCEL_DELIVERY_REQUEST_URL":"http://gateway.bx/cancel_delivery_request.php","HAS_CALLBACK_TRACKING_SUPPORT":"Y","CONFIG":[{"TYPE":"STRING","CODE":"SETTING_1","NAME":"String Example"},{"TYPE":"Y/N","CODE":"SETTING_2","NAME":"Checkbox Example"},{"TYPE":"NUMBER","CODE":"SETTING_3","NAME":"Number Example"},{"TYPE":"ENUM","CODE":"SETTING_4","NAME":"Enum Example","OPTIONS":{"Option1Code":"Option1Value","Option2Code":"Option2Value","Option3Code":"Option3Value","Option4Code":"Option4Value","Option5Code":"Option5Value"}},{"TYPE":"DATE","CODE":"SETTING_5","NAME":"Date Example"},{"TYPE":"LOCATION","CODE":"SETTING_6","NAME":"Location Example"}]},"PROFILES":[{"NAME":"Taxi","CODE":"TAXI","DESCRIPTION":"Taxi Delivery"},{"NAME":"Cargo","CODE":"CARGO","DESCRIPTION":"Cargo Delivery"}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.handler.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"uber","NAME":"Uber","DESCRIPTION":"Uber Description","SORT":250,"SETTINGS":{"CALCULATE_URL":"http://gateway.bx/calculate.php","CREATE_DELIVERY_REQUEST_URL":"http://gateway.bx/create_delivery_request.php","CANCEL_DELIVERY_REQUEST_URL":"http://gateway.bx/cancel_delivery_request.php","HAS_CALLBACK_TRACKING_SUPPORT":"Y","CONFIG":[{"TYPE":"STRING","CODE":"SETTING_1","NAME":"String Example"},{"TYPE":"Y/N","CODE":"SETTING_2","NAME":"Checkbox Example"},{"TYPE":"NUMBER","CODE":"SETTING_3","NAME":"Number Example"},{"TYPE":"ENUM","CODE":"SETTING_4","NAME":"Enum Example","OPTIONS":{"Option1Code":"Option1Value","Option2Code":"Option2Value","Option3Code":"Option3Value","Option4Code":"Option4Value","Option5Code":"Option5Value"}},{"TYPE":"DATE","CODE":"SETTING_5","NAME":"Date Example"},{"TYPE":"LOCATION","CODE":"SETTING_6","NAME":"Location Example"}]},"PROFILES":[{"NAME":"Taxi","CODE":"TAXI","DESCRIPTION":"Taxi Delivery"},{"NAME":"Cargo","CODE":"CARGO","DESCRIPTION":"Cargo Delivery"}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.handler.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.delivery.handler.add', {
    			CODE: "uber",
    			NAME: "Uber",
    			DESCRIPTION: "Uber Description",
    			SORT: 250,
    			SETTINGS: {
    				CALCULATE_URL: "http://gateway.bx/calculate.php",
    				CREATE_DELIVERY_REQUEST_URL: "http://gateway.bx/create_delivery_request.php",
    				CANCEL_DELIVERY_REQUEST_URL: "http://gateway.bx/cancel_delivery_request.php",
    				HAS_CALLBACK_TRACKING_SUPPORT: "Y",
    				CONFIG: [{
    						TYPE: "STRING",
    						CODE: "SETTING_1",
    						NAME: "String Example",
    					},
    					{
    						TYPE: "Y/N",
    						CODE: "SETTING_2",
    						NAME: "Checkbox Example",
    					},
    					{
    						TYPE: "NUMBER",
    						CODE: "SETTING_3",
    						NAME: "Number Example",
    					},
    					{
    						TYPE: "ENUM",
    						CODE: "SETTING_4",
    						NAME: "Enum Example",
    						OPTIONS: {
    							"Option1Code": "Option1Value",
    							"Option2Code": "Option2Value",
    							"Option3Code": "Option3Value",
    							"Option4Code": "Option4Value",
    							"Option5Code": "Option5Value",
    						},
    					},
    					{
    						TYPE: "DATE",
    						CODE: "SETTING_5",
    						NAME: "Date Example",
    					},
    					{
    						TYPE: "LOCATION",
    						CODE: "SETTING_6",
    						NAME: "Location Example",
    					},
    				],
    			},
    			PROFILES: [{
    					NAME: "Taxi",
    					CODE: "TAXI",
    					DESCRIPTION: "Taxi Delivery",
    				},
    				{
    					NAME: "Cargo",
    					CODE: "CARGO",
    					DESCRIPTION: "Cargo Delivery",
    				},
    			],
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
                'sale.delivery.handler.add',
                [
                    'CODE'        => "uber",
                    'NAME'        => "Uber",
                    'DESCRIPTION' => "Uber Description",
                    'SORT'        => 250,
                    'SETTINGS'    => [
                        'CALCULATE_URL'                => "http://gateway.bx/calculate.php",
                        'CREATE_DELIVERY_REQUEST_URL'  => "http://gateway.bx/create_delivery_request.php",
                        'CANCEL_DELIVERY_REQUEST_URL'  => "http://gateway.bx/cancel_delivery_request.php",
                        'HAS_CALLBACK_TRACKING_SUPPORT' => "Y",
                        'CONFIG'                       => [
                            [
                                'TYPE' => "STRING",
                                'CODE' => "SETTING_1",
                                'NAME' => "String Example",
                            ],
                            [
                                'TYPE' => "Y/N",
                                'CODE' => "SETTING_2",
                                'NAME' => "Checkbox Example",
                            ],
                            [
                                'TYPE' => "NUMBER",
                                'CODE' => "SETTING_3",
                                'NAME' => "Number Example",
                            ],
                            [
                                'TYPE'    => "ENUM",
                                'CODE'    => "SETTING_4",
                                'NAME'    => "Enum Example",
                                'OPTIONS' => [
                                    "Option1Code" => "Option1Value",
                                    "Option2Code" => "Option2Value",
                                    "Option3Code" => "Option3Value",
                                    "Option4Code" => "Option4Value",
                                    "Option5Code" => "Option5Value",
                                ],
                            ],
                            [
                                'TYPE' => "DATE",
                                'CODE' => "SETTING_5",
                                'NAME' => "Date Example",
                            ],
                            [
                                'TYPE' => "LOCATION",
                                'CODE' => "SETTING_6",
                                'NAME' => "Location Example",
                            ],
                        ],
                    ],
                    'PROFILES'    => [
                        [
                            'NAME'        => "Taxi",
                            'CODE'        => "TAXI",
                            'DESCRIPTION' => "Taxi Delivery",
                        ],
                        [
                            'NAME'        => "Cargo",
                            'CODE'        => "CARGO",
                            'DESCRIPTION' => "Cargo Delivery",
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
        echo 'Error adding delivery handler: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.delivery.handler.add', {
            CODE: "uber",
            NAME: "Uber",
            DESCRIPTION: "Uber Description",
            SORT: 250,
            SETTINGS: {
                CALCULATE_URL: "http://gateway.bx/calculate.php",
                CREATE_DELIVERY_REQUEST_URL: "http://gateway.bx/create_delivery_request.php",
                CANCEL_DELIVERY_REQUEST_URL: "http://gateway.bx/cancel_delivery_request.php",
                HAS_CALLBACK_TRACKING_SUPPORT: "Y",
                CONFIG: [{
                        TYPE: "STRING",
                        CODE: "SETTING_1",
                        NAME: "String Example",
                    },
                    {
                        TYPE: "Y/N",
                        CODE: "SETTING_2",
                        NAME: "Checkbox Example",
                    },
                    {
                        TYPE: "NUMBER",
                        CODE: "SETTING_3",
                        NAME: "Number Example",
                    },
                    {
                        TYPE: "ENUM",
                        CODE: "SETTING_4",
                        NAME: "Enum Example",
                        OPTIONS: {
                            "Option1Code": "Option1Value",
                            "Option2Code": "Option2Value",
                            "Option3Code": "Option3Value",
                            "Option4Code": "Option4Value",
                            "Option5Code": "Option5Value",
                        },
                    },
                    {
                        TYPE: "DATE",
                        CODE: "SETTING_5",
                        NAME: "Date Example",
                    },
                    {
                        TYPE: "LOCATION",
                        CODE: "SETTING_6",
                        NAME: "Location Example",
                    },
                ],
            },
            PROFILES: [{
                    NAME: "Taxi",
                    CODE: "TAXI",
                    DESCRIPTION: "Taxi Delivery",
                },
                {
                    NAME: "Cargo",
                    CODE: "CARGO",
                    DESCRIPTION: "Cargo Delivery",
                },
            ],
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
        'sale.delivery.handler.add',
        [
            'CODE' => "uber",
            'NAME' => "Uber",
            'DESCRIPTION' => "Uber Description",
            'SORT' => 250,
            'SETTINGS' => [
                'CALCULATE_URL' => "http://gateway.bx/calculate.php",
                'CREATE_DELIVERY_REQUEST_URL' => "http://gateway.bx/create_delivery_request.php",
                'CANCEL_DELIVERY_REQUEST_URL' => "http://gateway.bx/cancel_delivery_request.php",
                'HAS_CALLBACK_TRACKING_SUPPORT' => "Y",
                'CONFIG' => [
                    ['TYPE' => "STRING", 'CODE' => "SETTING_1", 'NAME' => "String Example"],
                    ['TYPE' => "Y/N", 'CODE' => "SETTING_2", 'NAME' => "Checkbox Example"],
                    ['TYPE' => "NUMBER", 'CODE' => "SETTING_3", 'NAME' => "Number Example"],
                    [
                        'TYPE' => "ENUM",
                        'CODE' => "SETTING_4",
                        'NAME' => "Enum Example",
                        'OPTIONS' => [
                            "Option1Code" => "Option1Value",
                            "Option2Code" => "Option2Value",
                            "Option3Code" => "Option3Value",
                            "Option4Code" => "Option4Value",
                            "Option5Code" => "Option5Value",
                        ],
                    ],
                    ['TYPE' => "DATE", 'CODE' => "SETTING_5", 'NAME' => "Date Example"],
                    ['TYPE' => "LOCATION", 'CODE' => "SETTING_6", 'NAME' => "Location Example"],
                ],
            ],
            'PROFILES' => [
                ['NAME' => "Taxi", 'CODE' => "TAXI", 'DESCRIPTION' => "Taxi Delivery"],
                ['NAME' => "Cargo", 'CODE' => "CARGO", 'DESCRIPTION' => "Cargo Delivery"],
            ],
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
   "result":14,
   "time":{
      "start":1713872092.378366,
      "finish":1713872092.691408,
      "duration":0.31304192543029785,
      "processing":0.015096187591552734,
      "date_start":"2024-04-23T14:34:52+02:00",
      "date_finish":"2024-04-23T14:34:52+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_delivery_handler.ID`](../../../data-types.md) | Identifier of the delivery service handler ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
   "error":"ERROR_CHECK_FAILURE",
   "error_description":"Parameter CODE is not defined"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ERROR_CHECK_FAILURE` | Validation error of incoming parameters (details in the error description) | 400 ||
|| `ERROR_HANDLER_ADD` | Error when trying to add a delivery service handler | 400 ||
|| `ERROR_HANDLER_ALREADY_EXIST` | A delivery service handler with the specified code `CODE` already exists | 400 ||
|| `ACCESS_DENIED` | Insufficient rights to add a delivery service handler | 403 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-handler-update.md)
- [{#T}](./sale-delivery-handler-delete.md)
- [{#T}](./sale-delivery-handler-list.md)
- [{#T}](../../../../tutorials/sale/delivery-in-crm.md)