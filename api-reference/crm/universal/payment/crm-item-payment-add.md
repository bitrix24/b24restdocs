# Add Payment crm.item.payment.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: requires access permission to modify the CRM object to which the payment is added.

The `crm.item.payment.add` method creates an empty payment with the amount `0` in a deal or invoice. Products and delivery are added to the payment by the [crm.item.payment.product.add](./products-in-payment/crm-item-payment-product-add.md) and [crm.item.payment.delivery.add](./delivery-in-payment/crm-item-payment-delivery-add.md) methods, and the payment amount is recalculated automatically.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityId***
[`integer`](../../../../api-reference/data-types.md) | Identifier of the deal or invoice. You can retrieve the identifier using the [crm.item.list](../crm-item-list.md) method ||
|| **entityTypeId***
[`integer`](../../../../api-reference/data-types.md) | Identifier of the [CRM object type](../../data-types.md#object_type). Payments support only two types:
- `2` — deal
- `31` — invoice

For other types, such as a lead `1` or a smart process, the method returns the `Cannot create payment for this entity.` error ||
|| **fields**
[`object`](../../../../api-reference/data-types.md) | Fields of the new payment. One key is supported:
- `paySystemId` — identifier of the payment system. You can retrieve the list of payment systems using the [sale.paysystem.list](../../../pay-system/sale-pay-system-list.md) method

If the parameter is not passed, the payment gets the default payment system. The method ignores other keys ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityId":13123,"entityTypeId":2}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.payment.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityId":13123,"entityTypeId":2,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.payment.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'crm.item.payment.add',
        params: {
          entityId: 13123,
          entityTypeId: 2,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created payment ID:', result)
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
      async function addPayment() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.payment.add',
            params: {
              entityId: 13123,
              entityTypeId: 2,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Created payment ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addPayment)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.item.payment.add(
            entity_id=13123,
            entity_type_id=2,
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
                'crm.item.payment.add',
                [
                    'entityId'     => 13123,
                    'entityTypeId' => 2,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding payment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.payment.add', {
            entityId: 13123,
            entityTypeId: 2
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
        'crm.item.payment.add',
        [
            'entityId' => 13123,
            'entityTypeId' => 2
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.item.payment.add", b24.Params{
    	"entityId":     13123,
    	"entityTypeId": 2,
    })
    if err != nil {
    	return fmt.Errorf("crm.item.payment.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 1033,
    "time": {
        "start": 1716193064.749158,
        "finish": 1716193065.656833,
        "duration": 0.90767502784729,
        "processing": 0.6450831890106201,
        "date_start": "2024-05-20T11:17:44+03:00",
        "date_finish": "2024-05-20T11:17:45+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`sale_order_payment.id`](../../../sale/data-types.md#sale_order_payment) | Identifier of the created payment ||
|| **time**
[`time`](../../../../api-reference/data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Cannot create payment for this entity."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Cannot create payment for this entity | The passed `entityTypeId` does not support payments. Payments are supported only for deals `2` and invoices `31` ||
|| `400` | `0` | Entity ID #`entityId` does not exist | The CRM object with the passed `entityId` was not found ||
|| `400` | `0` | You cannot create more payments on your current plan | The payment limit of the Bitrix24 plan has been reached ||
|| `400` | `ACCESS_DENIED` | Access denied | No permission to modify the CRM object, or a nonexistent `entityTypeId` was passed ||
|| `400` | `100` | Could not find value for parameter {`parameter name`} | The required parameter `entityId` or `entityTypeId` was not provided ||
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
- [{#T}](./crm-item-payment-update.md)