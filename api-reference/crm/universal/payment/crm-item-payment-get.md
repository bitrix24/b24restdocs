# Get Payment Information crm.item.payment.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: access permission to modify the payment order is required

This method retrieves brief information about the payment.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_payment.id`](../../../sale/data-types.md#sale_order_payment) | Payment identifier ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1036}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.payment.get
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1036,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.payment.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PaymentGetResult = {
      id: number
      accountNumber: string
      paid: 'Y' | 'N'
      datePaid: ISODate | null
      empPaidId: number
      paySystemId: number
      sum: number
      currency: string
      paySystemName: string
    }

    try {
      const response = await $b24.actions.v2.call.make<PaymentGetResult>({
        method: 'crm.item.payment.get',
        params: {
          id: 1036,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.id, result.accountNumber, result.paid, result.sum, result.currency)
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
      async function getPayment() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.payment.get',
            params: {
              id: 1036,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.id, result.accountNumber, result.paid, result.sum, result.currency)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getPayment)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.item.payment.get',
                [
                    'id' => 1036,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting payment item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.payment.get', {
            id: 1036,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.payment.get',
        [
            'id' => 1036
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
   "result":{
      "id":1036,
      "accountNumber":"3653\/1",
      "paid": "Y",
      "datePaid":"2024-05-20T12:32:02+02:00",
      "empPaidId":1,
      "paySystemId":6,
      "sum":0,
      "currency":"USD",
      "paySystemName":"Cash"
   },
   "time":{
      "start":1716203536.414886,
      "finish":1716203536.798211,
      "duration":0.38332509994506836,
      "processing":0.052394866943359375,
      "date_start":"2024-05-20T14:12:16+02:00",
      "date_finish":"2024-05-20T14:12:16+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_order_payment_crm_simple`](#sale_order_payment_crm_simple) | Object containing brief information about the payment  ||
|| **time**
[`time`](../../../../api-reference/data-types.md) | Information about the request execution time ||
|#

### Key result. Object of type sale_order_payment_crm_simple {#sale_order_payment_crm_simple}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`sale_order_payment.id`](../../../sale/data-types.md#sale_order_payment) | Payment identifier ||
|| **accountNumber**
[`string`](../../../../api-reference/data-types.md) | System payment number ||
|| **paid**
[`string`](../../../../api-reference/data-types.md) | Payment status

Possible values:
- `Y` – Paid
- `N` – Not paid ||
|| **datePaid**
[`datetime`](../../../../api-reference/data-types.md) | Payment date ||
|| **empPaidId**
[`user.id`](../../../../api-reference/data-types.md)| User who made the payment ||
|| **paySystemId**
[`sale_paysystem.id`](../../../sale/data-types.md#sale_paysystem) | Payment system identifier ||
|| **sum**
[`double`](../../../../api-reference/data-types.md) | Payment amount ||
|| **currency**
[`string`](../../../../api-reference/data-types.md) | Payment currency ||
|| **paySystemName**
[`string`](../../../../api-reference/data-types.md) | Name of the payment system ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":0,
   "error_description":"Insufficient permissions"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `0` | Payment not found or access denied ||
|| `100` | Parameter id not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-payment-update.md)
- [{#T}](./crm-item-payment-delete.md)
- [{#T}](./crm-item-payment-list.md)
- [{#T}](./crm-item-payment-pay.md)
- [{#T}](./crm-item-payment-unpay.md)
- [{#T}](./crm-item-payment-add.md)