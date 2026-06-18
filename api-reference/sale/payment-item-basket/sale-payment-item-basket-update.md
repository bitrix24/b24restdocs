# Update the binding of the cart item to the payment sale.paymentitembasket.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the binding of the cart item to the payment.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_payment_item_basket.id`](../data-types.md) | Identifier of the binding of the cart item to the payment ||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the binding of the cart item to the payment ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **quantity***
[`double`](../../data-types.md) | Quantity of the product ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier of the record ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1186,"fields":{"quantity":1,"xmlId":"myNewXmlId"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paymentitembasket.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1186,"fields":{"quantity":1,"xmlId":"myNewXmlId"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paymentitembasket.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type UpdatePaymentItemBasketResult = {
      paymentItemBasket: {
        basketId: number
        dateInsert: ISODate | null
        id: number
        paymentId: number
        quantity: number
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<UpdatePaymentItemBasketResult>({
        method: 'sale.paymentitembasket.update',
        params: {
          id: 1186,
          fields: {
            quantity: 1,
            xmlId: 'myNewXmlId',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.paymentItemBasket)
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
      async function updatePaymentItemBasket() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.paymentitembasket.update',
            params: {
              id: 1186,
              fields: {
                quantity: 1,
                xmlId: 'myNewXmlId',
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
          console.info(result.paymentItemBasket)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updatePaymentItemBasket)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.paymentitembasket.update',
                [
                    'id' => 1186,
                    'fields' => [
                        'quantity' => 1,
                        'xmlId' => 'myNewXmlId',
                    ],
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
        echo 'Error updating payment item basket: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.paymentitembasket.update', {
            id: 1186,
            fields: {
                quantity: 1,
                xmlId: 'myNewXmlId',
            }
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
        'sale.paymentitembasket.update',
        [
            'id' => 1186,
            'fields' =>
            [
                'quantity' => 1,
                'xmlId' => 'myNewXmlId',
            ]
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
        "paymentItemBasket": {
            "basketId": 2722,
            "dateInsert": "2024-04-17T09:37:45+02:00",
            "id": 1186,
            "paymentId": 1025,
            "quantity": 1,
            "xmlId": "myNewXmlId"
        }
    },
    "time": {
        "start": 1713341342.331169,
        "finish": 1713341343.013559,
        "duration": 0.6823902130126953,
        "processing": 0.4167962074279785,
        "date_start": "2024-04-17T11:09:02+02:00",
        "date_finish": "2024-04-17T11:09:03+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **paymentItemBasket**
[`sale_payment_item_basket`](../data-types.md) | Object with information about the updated binding of the cart item to the payment ||
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
|| `201240400001` | The updated binding of the cart item to the payment was not found ||
|| `200040300020` | Insufficient permissions to update the binding of the cart item to the payment ||
|| `100` | The `id` parameter is not specified ||
|| `100` | The `fields` parameter is not specified or is empty ||
|| `0` | Required fields in the `fields` structure are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-payment-item-basket-add.md)
- [{#T}](./sale-payment-item-basket-get.md)
- [{#T}](./sale-payment-item-basket-list.md)
- [{#T}](./sale-payment-item-basket-delete.md)
- [{#T}](./sale-payment-item-basket-get-fields.md)
