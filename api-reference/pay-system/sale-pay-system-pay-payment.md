# Pay for an Order via a Specific Payment System sale.paysystem.pay.payment

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`pay_system`](../scopes/permissions.md)
>
> Who can execute the method: a user with permissions to create and edit orders in CRM

This method is used to pay for an order through a specific payment system. It is called after processing the response from the payment system.

To perform the payment, there must be a payment associated with the specified payment system in the system.

## Method Parameters

{% include [Note on Required Parameters](../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **PAYMENT_ID*** 
[`sale_order_payment.id`](../sale/data-types.md) | Payment identifier
|| 
|| **PAY_SYSTEM_ID*** 
[`sale_paysystem.ID`](../sale/data-types.md) | Payment system identifier
|| 
|#

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PAYMENT_ID":1,"PAY_SYSTEM_ID":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paysystem.pay.payment
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PAYMENT_ID":1,"PAY_SYSTEM_ID":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paysystem.pay.payment
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
        method: 'sale.paysystem.pay.payment',
        params: {
          PAYMENT_ID: 1,
          PAY_SYSTEM_ID: 1,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Payment processed:', result)
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
      async function payPayment() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.paysystem.pay.payment',
            params: {
              PAYMENT_ID: 1,
              PAY_SYSTEM_ID: 1,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Payment processed:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', payPayment)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.paysystem.pay.payment',
                [
                    'PAYMENT_ID'    => 1,
                    'PAY_SYSTEM_ID' => 1,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling sale.paysystem.pay.payment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('sale.paysystem.pay.payment', {
            "PAYMENT_ID": 1,
            "PAY_SYSTEM_ID": 1,
        }, 
        function(result) 
        { 
            if (result.error()) 
            {
                console.error(result.error()); 
            }
            else 
            { 
                console.dir(result.data()); 
            } 
        } 
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.paysystem.pay.payment',
        [
            'PAYMENT_ID' => 1,
            'PAY_SYSTEM_ID' => 1
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
    "result": true,
    "time": {
        "start": 1712135335.026931,
        "finish": 1712135335.407762,
        "duration": 0.3808310031890869,
        "processing": 0.0336611270904541,
        "date_start": "2024-04-03T11:08:55+02:00",
        "date_finish": "2024-04-03T11:08:55+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Payment result ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_CHECK_FAILURE",
    "error_description": "Pay system not found"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Insufficient permissions to change the payment status | 403 ||
|| `ERROR_CHECK_FAILURE` | One of the required fields is missing, the specified payment system is not found, or the payment with the specified `ID` and payment system is not found (details can be found in the error description) | 400 ||
|| `ERROR_PROCESS_REQUEST_RESULT` | Error processing the request by the payment system (details can be found in the error description) | 400 ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-pay-system-handler-add.md)
- [{#T}](./sale-pay-system-handler-update.md)
- [{#T}](./sale-pay-system-handler-list.md)
- [{#T}](./sale-pay-system-handler-delete.md)
- [{#T}](./sale-pay-system-add.md)
- [{#T}](./sale-pay-system-update.md)
- [{#T}](./sale-pay-system-list.md)
- [{#T}](./sale-pay-system-delete.md)
- [{#T}](./sale-pay-system-settings-get.md)
- [{#T}](./sale-pay-system-settings-update.md)
- [{#T}](./sale-pay-system-settings-payment-get.md)
