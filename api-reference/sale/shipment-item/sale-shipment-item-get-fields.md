# Get Fields of Shipment Item from sale.shipmentitem.getfields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.shipmentitem.getfields` allows you to retrieve a list of available fields for shipment item table entries.

No parameters required.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipmentitem.get
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipmentitem.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type GetFieldsResult = {
      shipmentItem: Record<string, FieldDescription>
    }

    type FieldDescription = {
      isImmutable: boolean
      isReadOnly: boolean
      isRequired: boolean
      type: string
    }

    try {
      const response = await $b24.actions.v2.call.make<GetFieldsResult>({
        method: 'sale.shipmentitem.getfields',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.shipmentItem)
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
      async function getShipmentItemFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.shipmentitem.getfields',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.shipmentItem)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getShipmentItemFields)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.shipmentitem.getfields',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Info: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting shipment item fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.shipmentitem.getfields", {},
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
        'sale.shipmentitem.getfields',
        []
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
    "result": {
        "shipmentItem": {
            "basketId": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "type": "integer"
            },
            "dateInsert": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "datetime"
            },
            "id": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "integer"
            },
            "orderDeliveryId": {
                "isImmutable": true,
                "isReadOnly": false,
                "isRequired": true,
                "type": "integer"
            },
            "quantity": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": true,
                "type": "double"
            },
            "reservedQuantity": {
                "isImmutable": false,
                "isReadOnly": true,
                "isRequired": false,
                "type": "double"
            },
            "xmlId": {
                "isImmutable": false,
                "isReadOnly": false,
                "isRequired": false,
                "type": "string"
            }
        }
    },
    "time": {
        "start": 1712572880.529825,
        "finish": 1712572880.74746,
        "duration": 0.217634916305542,
        "processing": 0.0046880245208740234,
        "date_start": "2024-04-08T13:41:20+02:00",
        "date_finish": "2024-04-08T13:41:20+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **shipmentItem**
[`object`](../../data-types.md) | Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is the identifier of the object [`sale_order_shipment_item`](../data-types.md), and `value` is an object of type [`rest_field_description`](../data-types.md) ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 0,
    "error_description": "error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read available fields of the shipment item table ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./sale-shipment-item-add.md)
- [{#T}](./sale-shipment-item-update.md)
- [{#T}](./sale-shipment-item-get.md)
- [{#T}](./sale-shipment-item-list.md)
- [{#T}](./sale-shipment-item-delete.md)
