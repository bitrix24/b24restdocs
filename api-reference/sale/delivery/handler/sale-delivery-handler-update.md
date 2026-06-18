# Update Delivery Service Handler sale.delivery.handler.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

This method updates the delivery service handler.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`sale_delivery_handler.ID`](../../data-types.md) | Identifier of the delivery service handler.
You can obtain the identifiers of delivery service handlers using the [`sale.delivery.handler.list`](sale-delivery-handler-list.md) method.
  ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the delivery service handler ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the delivery service handler ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Description of the delivery service handler ||
|| **SETTINGS**
[`object`](../../../data-types.md) | An object containing information about the settings of the delivery service handler (detailed description provided [below](#parametr-settings))  ||
|| **PROFILES**
[`object[]`](../../../data-types.md) | An array containing a list of delivery profile objects (detailed description provided [below](#parametr-profiles)).

It is implied that the delivery service handler must have at least 1 profile ||
|#

### SETTINGS Parameter {#parametr-settings}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CALCULATE_URL***
[`string`](../../../data-types.md) | URL for calculating delivery cost.

This URL receives data about the parcel (what to deliver, where, and how), for which the delivery cost needs to be calculated in the response.

The request and response format is detailed in the documentation for the webhook **Calculate Delivery Cost**
 ||
|| **CREATE_DELIVERY_REQUEST_URL**
[`string`](../../../data-types.md) | URL for creating a delivery order.

This URL receives data about the parcel (what to deliver, where, and how), for which an order needs to be placed with the delivery service.

The request and response format is detailed in the documentation for the webhook **Create Delivery Order** ||
|| **CANCEL_DELIVERY_REQUEST_URL**
[`string`](../../../data-types.md) | URL for canceling a delivery order.

This URL receives data about the parcel (what to deliver, where, and how), for which the order needs to be canceled with the delivery service.

The request and response format is detailed in the documentation for the webhook **Cancel Delivery Order** ||
|| **HAS_CALLBACK_TRACKING_SUPPORT**
[`string`](../../../data-types.md) | Indicator of whether the delivery service will send notifications about the status of the delivery order (see method [`sale.delivery.request.sendmessage`](../delivery-request/sale-delivery-request-send-message.md)). 

If event support is indicated, a delivery activity will be created in the manager's interface when ordering delivery, into which changes related to the current delivery status can be transmitted.

Possible values:
- `Y` — support available
- `N` — no support
 ||
 || **CONFIG**
[`object`](../../../data-types.md) | An array of objects with available settings for the delivery service created using this handler (detailed description provided [below](#parametr-config)) ||
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
[`object`](../../../data-types.md) | List of options for selection. An object in the format `key=value`. Where the key is the option code, and the value is the option.

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'sale.delivery.handler.update',
        params: {
          ID: 14,
          CODE: 'uber',
          NAME: 'New Uber',
          DESCRIPTION: 'New Uber Description',
          SORT: 300,
          SETTINGS: {
            CALCULATE_URL: 'https://gateway.bx/calculate.php',
            CREATE_DELIVERY_REQUEST_URL: 'https://gateway.bx/create_delivery_request.php',
            CANCEL_DELIVERY_REQUEST_URL: 'https://gateway.bx/cancel_delivery_request.php',
            HAS_CALLBACK_TRACKING_SUPPORT: 'N',
            CONFIG: [
              { TYPE: 'STRING', CODE: 'SETTING_1', NAME: 'String Example' },
              { TYPE: 'Y/N', CODE: 'SETTING_2', NAME: 'Checkbox Example' },
              { TYPE: 'NUMBER', CODE: 'SETTING_3', NAME: 'Number Example' },
              {
                TYPE: 'ENUM',
                CODE: 'SETTING_4',
                NAME: 'Enum Example',
                OPTIONS: {
                  Option1Code: 'Option1Value',
                  Option2Code: 'Option2Value',
                  Option3Code: 'Option3Value',
                  Option4Code: 'Option4Value',
                  Option5Code: 'Option5Value',
                },
              },
              { TYPE: 'DATE', CODE: 'SETTING_5', NAME: 'Date Example' },
              { TYPE: 'LOCATION', CODE: 'SETTING_6', NAME: 'Location Example' },
            ],
          },
          PROFILES: [
            { NAME: 'New Taxi', CODE: 'TAXI', DESCRIPTION: 'New Taxi Delivery' },
            { NAME: 'New Cargo', CODE: 'CARGO', DESCRIPTION: 'New Cargo Delivery' },
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Handler updated successfully:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function updateDeliveryHandler() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.delivery.handler.update',
            params: {
              ID: 14,
              CODE: 'uber',
              NAME: 'New Uber',
              DESCRIPTION: 'New Uber Description',
              SORT: 300,
              SETTINGS: {
                CALCULATE_URL: 'https://gateway.bx/calculate.php',
                CREATE_DELIVERY_REQUEST_URL: 'https://gateway.bx/create_delivery_request.php',
                CANCEL_DELIVERY_REQUEST_URL: 'https://gateway.bx/cancel_delivery_request.php',
                HAS_CALLBACK_TRACKING_SUPPORT: 'N',
                CONFIG: [
                  { TYPE: 'STRING', CODE: 'SETTING_1', NAME: 'String Example' },
                  { TYPE: 'Y/N', CODE: 'SETTING_2', NAME: 'Checkbox Example' },
                  { TYPE: 'NUMBER', CODE: 'SETTING_3', NAME: 'Number Example' },
                  {
                    TYPE: 'ENUM',
                    CODE: 'SETTING_4',
                    NAME: 'Enum Example',
                    OPTIONS: {
                      Option1Code: 'Option1Value',
                      Option2Code: 'Option2Value',
                      Option3Code: 'Option3Value',
                      Option4Code: 'Option4Value',
                      Option5Code: 'Option5Value',
                    },
                  },
                  { TYPE: 'DATE', CODE: 'SETTING_5', NAME: 'Date Example' },
                  { TYPE: 'LOCATION', CODE: 'SETTING_6', NAME: 'Location Example' },
                ],
              },
              PROFILES: [
                { NAME: 'New Taxi', CODE: 'TAXI', DESCRIPTION: 'New Taxi Delivery' },
                { NAME: 'New Cargo', CODE: 'CARGO', DESCRIPTION: 'New Cargo Delivery' },
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Handler updated successfully:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateDeliveryHandler)
    </script>
    ```

- PHP


    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.delivery.handler.update',
                [
                    'ID'          => 14,
                    'CODE'        => "uber",
                    'NAME'        => "New Uber",
                    'DESCRIPTION' => "New Uber Description",
                    'SORT'        => 300,
                    'SETTINGS'    => [
                        'CALCULATE_URL'               => "https://gateway.bx/calculate.php",
                        'CREATE_DELIVERY_REQUEST_URL' => "https://gateway.bx/create_delivery_request.php",
                        'CANCEL_DELIVERY_REQUEST_URL' => "https://gateway.bx/cancel_delivery_request.php",
                        'HAS_CALLBACK_TRACKING_SUPPORT' => "N",
                        'CONFIG'                      => [
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
                            'NAME'        => "New Taxi",
                            'CODE'        => "TAXI",
                            'DESCRIPTION' => "New Taxi Delivery",
                        ],
                        [
                            'NAME'        => "New Cargo",
                            'CODE'        => "CARGO",
                            'DESCRIPTION' => "New Cargo Delivery",
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
        echo 'Error updating delivery handler: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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
      "date_start":"2024-04-23T10:33:08+02:00",
      "date_finish":"2024-04-23T10:33:09+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of updating the delivery service handler ||
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
|| `ERROR_HANDLER_NOT_FOUND` | Delivery service handler with the specified identifier not found  | 400 ||
|| `ERROR_CHECK_FAILURE` | Validation error of incoming parameters (details in the error description) | 400 ||
|| `ERROR_HANDLER_UPDATE` | Error while attempting to update the delivery service handler | 400 ||
|| `ERROR_HANDLER_ALREADY_EXIST` | Delivery service handler with the specified code already exists | 400 ||
|| `ACCESS_DENIED` | Insufficient rights to update the delivery service handler | 403 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-handler-add.md)
- [{#T}](./sale-delivery-handler-delete.md)
- [{#T}](./sale-delivery-handler-list.md)
