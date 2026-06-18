# Get Delivery Service Settings sale.delivery.config.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator

This method retrieves the settings of the delivery service.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`sale_delivery_service.ID`](../../data-types.md) | Identifier of the delivery service.
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
    -d '{"ID":196}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.config.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":196,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.config.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each setting returned in result[]
    type DeliveryConfigSetting = {
      CODE: string;
      VALUE: string;
    }

    try {
      const response = await $b24.actions.v2.call.make<DeliveryConfigSetting[]>({
        method: 'sale.delivery.config.get',
        params: {
          ID: 196,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Delivery config settings:', result.length, result)
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
      async function getDeliveryConfig() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.delivery.config.get',
            params: {
              ID: 196,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Delivery config settings:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getDeliveryConfig)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.delivery.config.get',
                [
                    'ID' => 196,
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
        echo 'Error getting delivery config: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.delivery.config.get', {
            ID: 196,
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
        'sale.delivery.config.get',
        [
            'ID' => 196
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
   "result":[
      {
         "CODE":"SETTING_1",
         "VALUE":"New SETTING_1 string value"
      },
      {
         "CODE":"SETTING_2",
         "VALUE":"N"
      },
      {
         "CODE":"SETTING_3",
         "VALUE":"999.99"
      },
      {
         "CODE":"SETTING_4",
         "VALUE":"Option2Code"
      },
      {
         "CODE":"SETTING_5",
         "VALUE":"25.03.2023"
      },
      {
         "CODE":"SETTING_6",
         "VALUE":"0000144962"
      }
   ],
   "time":{
      "start":1714137257.450324,
      "finish":1714137257.672526,
      "duration":0.22220182418823242,
      "processing":0.029755115509033203,
      "date_start":"2024-04-26T16:14:17+02:00",
      "date_finish":"2024-04-26T16:14:17+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../../data-types.md) | Values of the delivery service settings.
The structure of the settings (code, name, data type) is defined when creating or updating the delivery service handler using the methods:
- [sale.delivery.handler.add](../handler/sale-delivery-handler-add.md)
- [sale.delivery.handler.update](../handler/sale-delivery-handler-update.md) ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

### Key result

#|
|| **Name**
`type` | **Description** ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the setting ||
|| **VALUE**
[`any`](../../../data-types.md) | Value of the setting ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
   "error":"ERROR_DELIVERY_NOT_FOUND",
   "error_description":"Delivery not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ERROR_DELIVERY_NOT_FOUND` | Delivery service with the specified identifier (ID) not found | 400 ||
|| `ERROR_CHECK_FAILURE` | Validation error of incoming parameters (details in the error description) | 400 ||
|| `ACCESS_DENIED` | Insufficient rights to retrieve delivery service settings | 403 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-add.md)
- [{#T}](./sale-delivery-delete.md)
- [{#T}](./sale-delivery-update.md)
- [{#T}](./sale-delivery-config-update.md)
- [{#T}](./sale-delivery-get-list.md)
