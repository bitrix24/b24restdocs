# Get Payment Binding by Id sale.paymentItemShipment.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves the payment binding to the shipment by `ID`.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_payment_item_shipment.id`](../data-types.md) | Identifier of the payment binding to the shipment ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1183}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paymentitemshipment.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1183,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paymentitemshipment.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PaymentItemShipmentResult = {
      paymentItemShipment: {
        dateInsert: ISODate | null
        id: number
        paymentId: number
        shipmentId: number
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<PaymentItemShipmentResult>({
        method: 'sale.paymentitemshipment.get',
        params: {
          id: 1183,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.paymentItemShipment.id, result.paymentItemShipment.paymentId, result.paymentItemShipment.shipmentId)
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
      async function getPaymentItemShipment() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.paymentitemshipment.get',
            params: {
              id: 1183,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.paymentItemShipment.id, result.paymentItemShipment.paymentId, result.paymentItemShipment.shipmentId)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getPaymentItemShipment)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.paymentitemshipment.get',
                [
                    'id' => 1183,
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
        echo 'Error getting payment item shipment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.paymentitemshipment.get",
        {
            "id": 1183
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
        'sale.paymentitemshipment.get',
        [
            'id' => 1183
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
            "dateInsert": "2024-04-15T10:53:42+02:00",
            "id": 1183,
            "paymentId": 1025,
            "shipmentId": 2471,
            "xmlId": "myXmlId"
        }
    },
    "time": {
        "start": 1713171284.974848,
        "finish": 1713171285.256904,
        "duration": 0.2820558547973633,
        "processing": 0.035675048828125,
        "date_start": "2024-04-15T11:54:44+02:00",
        "date_finish": "2024-04-15T11:54:45+02:00"
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
[`sale_payment_item_shipment`](../data-types.md) | Information about the payment binding to the shipment ||
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
|| `201240400001` | Payment binding to the shipment not found ||
|| `200040300010` | Insufficient permissions to read payment binding data ||
|| `100` | Parameter `id` not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-payment-item-shipment-add.md)
- [{#T}](./sale-payment-item-shipment-update.md)
- [{#T}](./sale-payment-item-shipment-list.md)
- [{#T}](./sale-payment-item-shipment-delete.md)
- [{#T}](./sale-payment-item-shipment-get-fields.md)
