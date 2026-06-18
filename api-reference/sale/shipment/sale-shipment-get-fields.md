# Get Available Shipment Fields sale.shipment.getfields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves the available shipment fields.

No parameters.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipment.getfields
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipment.getfields
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldDescription = {
      isImmutable: boolean
      isReadOnly: boolean
      isRequired: boolean
      type: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ShipmentGetFieldsResult = {
      shipment: Record<string, FieldDescription>
    }

    try {
      const response = await $b24.actions.v2.call.make<ShipmentGetFieldsResult>({
        method: 'sale.shipment.getfields',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(Object.keys(result.shipment), result.shipment)
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
      async function getShipmentFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.shipment.getfields',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(Object.keys(result.shipment), result.shipment)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getShipmentFields)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.shipment.getfields',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting shipment fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.shipment.getfields", {},
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
        'sale.shipment.getfields',
        []
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
   "result":{
      "shipment":{
         "accountNumber":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "allowDelivery":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":true,
            "type":"char"
         },
         "basePriceDelivery":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"double"
         },
         "canceled":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"char"
         },
         "comments":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"string"
         },
         "companyId":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"integer"
         },
         "currency":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "customPriceDelivery":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"char"
         },
         "dateAllowDelivery":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"datetime"
         },
         "dateCanceled":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"datetime"
         },
         "dateDeducted":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"datetime"
         },
         "dateInsert":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"datetime"
         },
         "dateMarked":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"datetime"
         },
         "dateResponsibleId":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"datetime"
         },
         "deducted":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":true,
            "type":"char"
         },
         "deliveryDocDate":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"datetime"
         },
         "deliveryDocNum":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"string"
         },
         "deliveryId":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":true,
            "type":"integer"
         },
         "deliveryName":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "deliveryXmlId":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "discountPrice":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"double"
         },
         "empAllowDeliveryId":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"integer"
         },
         "empCanceledId":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"integer"
         },
         "empDeductedId":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"integer"
         },
         "empMarkedId":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"integer"
         },
         "empResponsibleId":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"integer"
         },
         "externalDelivery":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"char"
         },
         "id":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"integer"
         },
         "id1c":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "marked":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"char"
         },
         "orderId":{
            "isImmutable":true,
            "isReadOnly":false,
            "isRequired":true,
            "type":"integer"
         },
         "priceDelivery":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"double"
         },
         "reasonMarked":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "reasonUndoDeducted":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "responsibleId":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"integer"
         },
         "statusId":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"char"
         },
         "statusXmlId":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"char"
         },
         "system":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"char"
         },
         "trackingDescription":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "trackingLastCheck":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "trackingNumber":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"string"
         },
         "trackingStatus":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "updated1c":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"char"
         },
         "version1c":{
            "isImmutable":false,
            "isReadOnly":true,
            "isRequired":false,
            "type":"string"
         },
         "xmlId":{
            "isImmutable":false,
            "isReadOnly":false,
            "isRequired":false,
            "type":"string"
         }
      }
   },
   "time":{
      "start":1713190893.141474,
      "finish":1713190895.854316,
      "duration":2.7128419876098633,
      "processing":0.16173791885375977,
      "date_start":"2024-04-15T17:21:33+02:00",
      "date_finish":"2024-04-15T17:21:35+02:00"
   }
}
```
### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **shipment**
[`object`](../../data-types.md) | Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is the identifier of the object [`sale_order_shipment`](../data-types.md#sale_order_shipment), and `value` is an object of type [`rest_field_description`](../data-types.md#rest_field_description) ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":0,
   "error_description":"error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read available shipment fields ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-shipment-add.md)
- [{#T}](./sale-shipment-update.md)
- [{#T}](./sale-shipment-get.md)
- [{#T}](./sale-shipment-list.md)
- [{#T}](./sale-shipment-delete.md)
