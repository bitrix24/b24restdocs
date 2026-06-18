# Delete Delivery Request sale.delivery.request.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale, delivery`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a delivery request.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **DELIVERY_ID*** 
[`sale_delivery_service.ID`](../../data-types.md) | Identifier of the delivery service to which the delivery request belongs.

You can obtain the identifiers of `sale_delivery_service.ID` for delivery services using the [sale.delivery.getlist](../delivery/sale-delivery-get-list.md) method.
|| 
|| **REQUEST_ID*** 
[`string`](../../../data-types.md) | Identifier of the delivery request.

The identifier is assigned by the external system in response to the webhook for creating a delivery order (more details in the webhook description [Creating a Delivery Order](../webhooks/create-delivery-request.md)).
|| 
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DELIVERY_ID":225,"REQUEST_ID":"4757aca4931a4f029f49c0db4374d13d"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.delivery.request.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"DELIVERY_ID":225,"REQUEST_ID":"4757aca4931a4f029f49c0db4374d13d","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.delivery.request.delete
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
        method: 'sale.delivery.request.delete',
        params: {
          DELIVERY_ID: 225,
          REQUEST_ID: '4757aca4931a4f029f49c0db4374d13d',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Delivery request deleted:', result)
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
      async function deleteDeliveryRequest() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.delivery.request.delete',
            params: {
              DELIVERY_ID: 225,
              REQUEST_ID: '4757aca4931a4f029f49c0db4374d13d',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Delivery request deleted:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteDeliveryRequest)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.delivery.request.delete',
                [
                    'DELIVERY_ID' => 225,
                    'REQUEST_ID'  => "4757aca4931a4f029f49c0db4374d13d",
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
        echo 'Error deleting delivery request: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.delivery.request.delete', {
            DELIVERY_ID: 225,
            REQUEST_ID: "4757aca4931a4f029f49c0db4374d13d",
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
        'sale.delivery.request.delete',
        [
            'DELIVERY_ID' => 225,
            'REQUEST_ID' => "4757aca4931a4f029f49c0db4374d13d"
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
        "start":1714549724.272976,
        "finish":1714549724.479944,
        "duration":0.20696806907653809,
        "processing":0.02615499496459961,
        "date_start":"2024-05-01T10:48:44+02:00",
        "date_finish":"2024-05-01T10:48:44+02:00"
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the delivery request deletion ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

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
|| `DELETE_REQUEST_INTERNAL_ERROR` | Error while attempting to delete the delivery request | `400` ||
|| `ACCESS_DENIED` | Insufficient permissions to delete the delivery request | `403` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-delivery-request-update.md)
- [{#T}](./sale-delivery-request-send-message.md)
