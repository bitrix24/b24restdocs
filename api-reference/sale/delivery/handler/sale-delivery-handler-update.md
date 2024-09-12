# Update Delivery Handler sale.delivery.handler.update

> Scope: [`sale`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

This method updates the delivery handler.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`sale_delivery_handler.ID`](../../data-types.md) | Identifier of the delivery handler.
You can obtain the identifiers of delivery handlers using the method [`sale.delivery.handler.list`](sale-delivery-handler-list.md)
  ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the delivery handler ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the delivery handler ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Description of the delivery handler ||
|| **SETTINGS**
[`object`](../../../data-types.md) | Object containing information about the settings of the delivery handler (detailed description provided [below](#parametr-settings))  ||
|| **PROFILES**
[`object[]`](../../../data-types.md) | Array containing a list of delivery profile objects (detailed description provided [below](#parametr-profiles)).

It is implied that the delivery handler must have at least 1 profile ||
|#

### SETTINGS Parameter {#parametr-settings}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CALCULATE_URL***
[`string`](../../../data-types.md) | URL for calculating delivery costs.

Data about the package (what to deliver, where, and how) is sent to this URL, for which the delivery cost needs to be calculated in the response.

The request and response format is detailed in the documentation for the webhook **Calculate Delivery Cost**
 ||
|| **CREATE_DELIVERY_REQUEST_URL**
[`string`](../../../data-types.md) | URL for creating a delivery order.

Data about the package (what to deliver, where, and how) is sent to this URL, for which an order needs to be placed with the delivery service.

The request and response format is detailed in the documentation for the webhook **Create Delivery Order** ||
|| **CANCEL_DELIVERY_REQUEST_URL**
[`string`](../../../data-types.md) | URL for canceling a delivery order.

Data about the package (what to deliver, where, and how) is sent to this URL, for which the order needs to be canceled with the delivery service.

The request and response format is detailed in the documentation for the webhook **Cancel Delivery Order** ||
|| **HAS_CALLBACK_TRACKING_SUPPORT**
[`string`](../../../data-types.md) | Indicator of whether the delivery service will send notifications about the status of the delivery order (see method [`sale.delivery.request.sendmessage`](../delivery-request/sale-delivery-request-send-message.md)). 

If event support is indicated, a CRM activity will be created in the manager's interface when ordering delivery, into which changes related to the current delivery status can be transmitted.

Possible values:
- `Y` — support available
- `N` — no support
 ||
 || **CONFIG**
[`object`](../../../data-types.md) | Array of objects with available settings for the delivery service created using this handler (detailed description provided [below](#parametr-config)) ||
|#

### CONFIG Parameter {#parametr-config}

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
[`object`](../../../data-types.md) | List of options for selection. Object in the format `key=value`. Where the key is the option code, and the value is the option.

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

### PROFILES Parameter {#parametr-profiles}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
[`string`](../../../data-types.md) | Name of the delivery handler profile  ||
|| **CODE***
[`string`](../../../data-types.md) | Symbolic code of the delivery handler profile ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Description of the delivery handler profile  ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":14,"CODE":"uber","NAME":"New Uber","DESCRIPTION":"New Uber Description","SORT":300,"SETTINGS":{"CALCULATE_URL":"https://gateway.bx/calculate.php","CREATE_DELIVERY_REQUEST_URL":"https://gateway.bx/create_delivery_request.php","CANCEL_DELIVERY_REQUEST_URL":"https://gateway.bx/cancel_delivery_request.php","HAS_CALLBACK_TRACKING_SUPPORT":"N","CONFIG":[{"TYPE":"STRING","CODE":"SETTING_1","NAME":"String Example"},{"TYPE":"Y/N","CODE":"SETTING_2","NAME":"Checkbox Example"},{"TYPE":"NUMBER","CODE":"SETTING_3","NAME":"Number Example"},{"TYPE":"ENUM","CODE":"SETTING_4","NAME":"Enum Example","OPTIONS":{"Option1Code":"Option1Value","Option2Code":"Option2Value","Option3Code":"Option3Value","Option4Code":"Option4Value","Option5Code":"Option5Value"}},{"TYPE":"DATE","CODE":"SETTING_5","NAME":"Date Example"},{"TYPE":"LOCATION","CODE":"SETTING_6","NAME":"Location Example"}]},"PROFILES":[{"NAME":"New Taxi","CODE":"TAXI","DESCRIPTION":"New Taxi Delivery"},{"NAME":"New Cargo","CODE":"CARGO","DESCRIPTION":"New Cargo Delivery"}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.handler.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":14,"CODE":"uber","NAME":"New Uber","DESCRIPTION":"New Uber Description","SORT":300,"SETTINGS":{"CALCULATE_URL":"https://gateway.bx/calculate.php","CREATE_DELIVERY_REQUEST_URL":"https://gateway.bx/create_delivery_request.php","CANCEL_DELIVERY_REQUEST_URL":"https://gateway.bx/cancel_delivery_request.php","HAS_CALLBACK_TRACKING_SUPPORT":"N","CONFIG":[{"TYPE":"STRING","CODE":"SETTING_1","NAME":"String Example"},{"TYPE":"Y/N","CODE":"SETTING_2","NAME":"Checkbox Example"},{"TYPE":"NUMBER","CODE":"SETTING_3","NAME":"Number Example"},{"TYPE":"ENUM","CODE":"SETTING_4","NAME":"Enum Example","OPTIONS":{"Option1Code":"Option1Value","Option2Code":"Option2Value","Option3Code":"Option3Value","Option4Code":"Option4Value","Option5Code":"Option5Value"}},{"TYPE":"DATE","CODE":"SETTING_5","NAME":"Date Example"},{"TYPE":"LOCATION","CODE":"SETTING_6","NAME":"Location Example"}]},"PROFILES":[{"NAME":"New Taxi","CODE":"TAXI","DESCRIPTION":"New Taxi Delivery"},{"NAME":"New Cargo","CODE":"CARGO","DESCRIPTION":"New Cargo Delivery"}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.handler.update
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.delivery.handler.update', {
            ID: 14,
            CODE: "uber",
            NAME: "New Uber",
            DESCRIPTION: "New Uber Description",
            SORT: 300,
            SETTINGS: {
                CALCULATE_URL: "https://gateway.bx/calculate.php",
                CREATE_DELIVERY_REQUEST_URL: "https://gateway.bx/create_delivery_request.php",
                CANCEL_DELIVERY_REQUEST_URL: "https://gateway.bx/cancel_delivery_request.php",
                HAS_CALLBACK_TRACKING_SUPPORT: "N",
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
                    NAME: "New Taxi",
                    CODE: "TAXI",
                    DESCRIPTION: "New Taxi Delivery",
                },
                {
                    NAME: "New Cargo",
                    CODE: "CARGO",
                    DESCRIPTION: "New Cargo Delivery",
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

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.delivery.handler.update',
        [
            'ID' => 14,
            'CODE' => "uber",
            'NAME' => "New Uber",
            'DESCRIPTION' => "New Uber Description",
            'SORT' => 300,
            'SETTINGS' => [
                'CALCULATE_URL' => "https://gateway.bx/calculate.php",
                'CREATE_DELIVERY_REQUEST_URL' => "https://gateway.bx/create_delivery_request.php",
                'CANCEL_DELIVERY_REQUEST_URL' => "https://gateway.bx/cancel_delivery_request.php",
                'HAS_CALLBACK_TRACKING_SUPPORT' => "N",
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
                ['NAME' => "New Taxi", 'CODE' => "TAXI", 'DESCRIPTION' => "New Taxi Delivery"],
                ['NAME' => "New Cargo", 'CODE' => "CARGO", 'DESCRIPTION' => "New Cargo Delivery"],
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
   "result":true,
   "time":{
      "start":1713857588.711742,
      "finish":1713857589.121912,
      "duration":0.4101700782775879,
      "processing":0.02155303955078125,
      "date_start":"2024-04-23T10:33:08+03:00",
      "date_finish":"2024-04-23T10:33:09+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of updating the delivery handler ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
   "error":"ERROR_HANDLER_NOT_FOUND",
   "error_description":"Handler not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ERROR_HANDLER_NOT_FOUND` | Delivery handler with the specified identifier not found  | 400 ||
|| `ERROR_CHECK_FAILURE` | Validation error of incoming parameters (details in the error description) | 400 ||
|| `ERROR_HANDLER_UPDATE` | Error while attempting to update the delivery handler | 400 ||
|| `ERROR_HANDLER_ALREADY_EXIST` | Delivery handler with the specified code already exists | 400 ||
|| `ACCESS_DENIED` | Insufficient permissions to update the delivery handler | 403 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-handler-add.md)
- [{#T}](./sale-delivery-handler-delete.md)
- [{#T}](./sale-delivery-handler-list.md)