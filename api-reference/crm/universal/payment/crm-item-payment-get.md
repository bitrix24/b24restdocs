# Get Payment Information crm.item.payment.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to read the order the payment belongs to

The `crm.item.payment.get` method returns the status, amount, and payment system of a payment by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_payment.id`](../../../sale/data-types.md#sale_order_payment) | Payment identifier. You can retrieve the payment identifiers of a CRM object using the [crm.item.payment.list](./crm-item-payment-list.md) method ||
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

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.item.payment.get(
            bitrix_id=1036,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.item.payment.get", b24.Params{
    	"id": 1036,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.item.payment.get: %w", err)
    }

    var item struct {
    	ID            b24.ID `json:"id"`
    	AccountNumber string `json:"accountNumber"`
    	Paid          string `json:"paid"`
    	DatePaid      string `json:"datePaid"`
    	EmpPaidID     b24.ID `json:"empPaidId"`
    	PaySystemID   b24.ID `json:"paySystemId"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.AccountNumber)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "id": 1036,
        "accountNumber": "1049/1",
        "paid": "Y",
        "datePaid": "2026-10-28T16:59:33+01:00",
        "empPaidId": 1,
        "paySystemId": 11,
        "sum": 3000,
        "currency": "USD",
        "paySystemName": "Cash"
    },
    "time": {
        "start": 1793203175,
        "finish": 1793203175.965445,
        "duration": 0.9654450416564941,
        "processing": 0,
        "date_start": "2026-10-28T16:59:35+01:00",
        "date_finish": "2026-10-28T16:59:35+01:00",
        "operating_reset_at": 1793203740,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_order_payment_crm_simple`](#sale_order_payment_crm_simple) | Payment data [(detailed description)](#sale_order_payment_crm_simple) ||
|| **time**
[`time`](../../../../api-reference/data-types.md) | Information about the request execution time ||
|#

#### sale_order_payment_crm_simple {#sale_order_payment_crm_simple}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`sale_order_payment.id`](../../../sale/data-types.md#sale_order_payment) | Payment identifier ||
|| **accountNumber**
[`string`](../../../../api-reference/data-types.md) | Payment number in the `order number/payment sequence number` format, for example `1045/1` ||
|| **paid**
[`string`](../../../../api-reference/data-types.md) | Payment status:
- `Y` — paid
- `N` — not paid ||
|| **datePaid**
[`datetime`](../../../../api-reference/data-types.md) | Date and time when the payment was last marked as paid. `null` if the payment has never been made.

The value is kept after [crm.item.payment.unpay](./crm-item-payment-unpay.md) ||
|| **empPaidId**
[`user.id`](../../../../api-reference/data-types.md) | Identifier of the user who last marked the payment as paid. `null` if the payment has never been made.

The value is kept after [crm.item.payment.unpay](./crm-item-payment-unpay.md) ||
|| **paySystemId**
[`sale_paysystem.id`](../../../sale/data-types.md#sale_paysystem) | Payment system identifier ||
|| **sum**
[`double`](../../../../api-reference/data-types.md) | Payment amount. It is recalculated automatically when products and delivery are added to or removed from the payment ||
|| **currency**
[`string`](../../../../api-reference/data-types.md) | Payment currency ||
|| **paySystemName**
[`string`](../../../../api-reference/data-types.md) | Name of the payment system ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Payment has not been found"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Payment has not been found | The payment with the passed `id` was not found ||
|| `400` | `ACCESS_DENIED` | Access denied | No permission to read the order the payment belongs to ||
|| `400` | `100` | Could not find value for parameter {id} | The required parameter `id` was not provided ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./products-in-payment/index.md)
- [{#T}](./delivery-in-payment/index.md)
- [{#T}](./crm-item-payment-update.md)
- [{#T}](./crm-item-payment-delete.md)
- [{#T}](./crm-item-payment-list.md)
- [{#T}](./crm-item-payment-pay.md)
- [{#T}](./crm-item-payment-unpay.md)
- [{#T}](./crm-item-payment-add.md)