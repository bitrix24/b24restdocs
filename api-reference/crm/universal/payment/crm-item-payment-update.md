# Update Payment Fields crm.item.payment.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify the order the payment belongs to

The `crm.item.payment.update` method changes the payment status and payment system. Other payment fields can be changed using the [sale.payment.update](../../../sale/payment/sale-payment-update.md) method.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_payment.id`](../../../sale/data-types.md#sale_order_payment) | Payment identifier. You can retrieve the payment identifiers of a CRM object using the [crm.item.payment.list](./crm-item-payment-list.md) method ||
|| **fields***
[`object`](../../../../api-reference/data-types.md) | Payment fields to change [(detailed description)](#fields). Fields that are not passed keep their current values ||
|#

### Parameter fields {#fields}

The method accepts only two fields. Other keys are ignored. If `fields` contains no supported key, the method returns the `Empty fields` error.

#|
|| **Name**
`type` | **Description** ||
|| **paid**
[`string`](../../../../api-reference/data-types.md) | Payment status:
- `Y` — paid
- `N` — not paid

Works the same way as the [crm.item.payment.pay](./crm-item-payment-pay.md) and [crm.item.payment.unpay](./crm-item-payment-unpay.md) methods ||
|| **paySystemId**
[`sale_paysystem.id`](../../../sale/data-types.md#sale_paysystem) | Payment system identifier. You can retrieve the list of payment systems using the [sale.paysystem.list](../../../pay-system/sale-pay-system-list.md) method.

The payment system name in the `paySystemName` field of the payment is updated automatically ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1036,"fields":{"paid":"Y","paySystemId":110}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.payment.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1036,"fields":{"paid":"Y","paySystemId":110},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.payment.update
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
        method: 'crm.item.payment.update',
        params: {
          id: 1036,
          fields: {
            paid: 'Y',
            paySystemId: 110,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Payment updated:', result)
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
      async function updatePayment() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.payment.update',
            params: {
              id: 1036,
              fields: {
                paid: 'Y',
                paySystemId: 110,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Payment updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updatePayment)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.item.payment.update(
            bitrix_id=1036,
            fields={
                "paid": "Y",
                "paySystemId": 110,
            },
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
                'crm.item.payment.update',
                [
                    'id' => 1036,
                    'fields' => [
                        'paid' => "Y",
                        'paySystemId' => 110,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating payment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.payment.update', {
            id: 1036,
            fields: {
                paid: "Y",
                paySystemId: 110,
            }
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
        'crm.item.payment.update',
        [
            'id' => 1036,
            'fields' => [
                'paid' => 'Y',
                'paySystemId' => 110,
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.item.payment.update", b24.Params{
    	"id": 1036,
    	"fields": b24.Params{
    		"paid":        "Y",
    		"paySystemId": 110,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.item.payment.update: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
   "result":true,
   "time":{
      "start":1716212943.651507,
      "finish":1716212944.578154,
      "duration":0.9266471862792969,
      "processing":0.6742370128631592,
      "date_start":"2024-05-20T16:49:03+03:00",
      "date_finish":"2024-05-20T16:49:04+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../../api-reference/data-types.md) | `true` if the payment has been updated ||
|| **time**
[`time`](../../../../api-reference/data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Empty fields"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Empty fields | `fields` contains neither `paid` nor `paySystemId` ||
|| `400` | `0` | Payment system has not been found | The payment system with the passed `paySystemId` was not found ||
|| `400` | `0` | Payment has not been found | The payment with the passed `id` was not found ||
|| `400` | `BX_ERROR` | Insufficient funds on internal account | `paid` = `Y` was passed, the payment uses the "Internal account" payment system, and the client's internal account does not have enough funds ||
|| `400` | `ACCESS_DENIED` | Access denied | No permission to modify the order the payment belongs to ||
|| `400` | `100` | Could not find value for parameter {`parameter name`} | The required parameter `id` or `fields` was not provided ||
|#

The method returns the code `0` for several different errors. To determine the cause, check the text in `error_description`.

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./products-in-payment/index.md)
- [{#T}](./delivery-in-payment/index.md)
- [{#T}](./crm-item-payment-delete.md)
- [{#T}](./crm-item-payment-get.md)
- [{#T}](./crm-item-payment-list.md)
- [{#T}](./crm-item-payment-pay.md)
- [{#T}](./crm-item-payment-unpay.md)
- [{#T}](./crm-item-payment-add.md)