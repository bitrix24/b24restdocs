# Adding a Product Item to Payment crm.item.payment.product.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the CRM object associated with the payment. The payment must not be paid

The method `crm.item.payment.product.add` adds a product from a deal or invoice product row to a payment. The payment amount increases by the product price multiplied by `quantity`.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **paymentId***
[`sale_order_payment.id`](../../../../sale/data-types.md#sale_order_payment) | Identifier of the payment. You can retrieve the payment identifiers of a CRM object using the [crm.item.payment.list](../crm-item-payment-list.md) method ||
|| **rowId***
[`integer`](../../../../data-types.md) | Identifier of the product row of the deal or invoice that the payment belongs to. You can retrieve it using the [crm.item.productrow.list](../../product-rows/crm-item-productrow-list.md) method — the `id` field in the response ||
|| **quantity***
[`integer`](../../../../data-types.md) | Product quantity — an integer from `1` to the unallocated remainder of the product row. The remainder is the quantity in the product row minus the quantity of this product in all payments of the order.

If the product has already been fully allocated to other payments, the method returns the `Product was not found.` error ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"paymentId":1039,"rowId":17587,"quantity":2}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.payment.product.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"paymentId":1039,"rowId":17587,"quantity":2,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.payment.product.add
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
        method: 'crm.item.payment.product.add',
        params: {
          paymentId: 1039,
          rowId: 17587,
          quantity: 2,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Added product row id:', result)
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
      async function addPaymentProduct() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.payment.product.add',
            params: {
              paymentId: 1039,
              rowId: 17587,
              quantity: 2,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Added product row id:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addPaymentProduct)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.item.payment.product.add(
            payment_id=1039,
            row_id=17587,
            quantity=2,
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
        print(f"Unexpected Error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.item.payment.product.add',
                [
                    'paymentId' => 1039,
                    'rowId'     => 17587,
                    'quantity'  => 2
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding payment product: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.payment.product.add', {
            paymentId: 1039,
            rowId: 17587,
            quantity: 2
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
        'crm.item.payment.product.add',
        [
            'paymentId' => 1039,
            'rowId' => 17587,
            'quantity' => 2
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.item.payment.product.add", b24.Params{
    	"paymentId": 1039,
    	"rowId":     17587,
    	"quantity":  2,
    })
    if err != nil {
    	return fmt.Errorf("crm.item.payment.product.add: %w", err)
    }

    var id int
    if err := json.Unmarshal(res.Result, &id); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("item ID:", id)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
   "result":1193,
   "time":{
      "start":1716276648.349503,
      "finish":1716276649.261574,
      "duration":0.9120709896087646,
      "processing":0.6422691345214844,
      "date_start":"2024-05-21T10:30:48+03:00",
      "date_finish":"2024-05-21T10:30:49+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../../data-types.md) | Identifier of the created product item in the payment. Use it in the [crm.item.payment.product.setQuantity](./crm-item-payment-product-set-quantity.md) and [crm.item.payment.product.delete](./crm-item-payment-product-delete.md) methods ||
|| **time**
[`time`](../../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Insufficient product stock."
}
```

{% include notitle [Error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Product quantity must be greater than zero | A value of `0` or less was passed in `quantity` ||
|| `400` | `0` | Insufficient product stock | The total quantity of this product in all payments would exceed the quantity in the product row of the CRM object ||
|| `400` | `0` | Product was not found | The CRM object that the payment belongs to has no product row with the passed `rowId`, or the product has already been fully allocated to payments ||
|| `400` | `0` | Payment has not been found | The payment with the passed `paymentId` was not found ||
|| `400` | `ACCESS_DENIED` | Access denied | The payment is already paid, or the user does not have permission to modify the payment. To change the contents of a paid payment, first call [crm.item.payment.unpay](../crm-item-payment-unpay.md) ||
|| `400` | `0` | Order binding has not been found | The payment's order is not linked to a CRM object ||
|| `400` | `0` | Entity does not support links with catalog products | The CRM object that the payment belongs to does not support products ||
|| `400` | `100` | Could not find value for parameter {`parameter name`} | The required parameter `paymentId`, `rowId`, or `quantity` was not provided ||
|#

The method returns code `0` for several different errors. To determine the cause, check the text in `error_description`.

{% include [System errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-payment-product-list.md)
- [{#T}](./crm-item-payment-product-delete.md)
- [{#T}](./crm-item-payment-product-set-quantity.md)
