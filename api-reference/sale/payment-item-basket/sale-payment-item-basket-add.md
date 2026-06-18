# Add Basket Item Binding to Payment sale.paymentitembasket.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a binding of a basket item to a payment.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating a binding of a basket item to a payment ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **paymentId***
[`sale_order_payment.id`](../data-types.md) | Payment identifier ||
|| **basketId***
[`sale_basket_item.id`](../data-types.md) | Basket item identifier ||
|| **quantity**
[`double`](../../data-types.md) | Quantity of the product ||
|| **xmlId**
[`string`](../../data-types.md) | External record identifier ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"quantity":3,"basketId":2722,"paymentId":1025,"xmlId":"myXmlId"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.paymentitembasket.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"quantity":3,"basketId":2722,"paymentId":1025,"xmlId":"myXmlId"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.paymentitembasket.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AddPaymentItemBasketResult = {
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
      const response = await $b24.actions.v2.call.make<AddPaymentItemBasketResult>({
        method: 'sale.paymentitembasket.add',
        params: {
          fields: {
            quantity: 3,
            basketId: 2722,
            paymentId: 1025,
            xmlId: 'myXmlId',
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
      async function addPaymentItemBasket() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.paymentitembasket.add',
            params: {
              fields: {
                quantity: 3,
                basketId: 2722,
                paymentId: 1025,
                xmlId: 'myXmlId',
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

      document.addEventListener('DOMContentLoaded', addPaymentItemBasket)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.paymentitembasket.add',
                [
                    'fields' => [
                        'quantity'  => 3,
                        'basketId'  => 2722,
                        'paymentId' => 1025,
                        'xmlId'     => 'myXmlId',
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
        echo 'Error adding payment item to basket: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.paymentitembasket.add', {
            fields: {
                quantity: 3,
                basketId: 2722,
                paymentId: 1025,
                xmlId: 'myXmlId',
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
        'sale.paymentitembasket.add',
        [
            'fields' =>
            [
                'quantity' => 3,
                'basketId' => 2722,
                'paymentId' => 1025,
                'xmlId' => 'myXmlId',
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
            "quantity": 3,
            "xmlId": "myXmlId"
        }
    },
    "time": {
        "start": 1713339465.349855,
        "finish": 1713339465.968993,
        "duration": 0.6191380023956299,
        "processing": 0.38312196731567383,
        "date_start": "2024-04-17T10:37:45+02:00",
        "date_finish": "2024-04-17T10:37:45+02:00"
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
[`sale_payment_item_basket`](../data-types.md) | Object with information about the added binding of the basket item to the payment ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 201240400002,
    "error_description": "payment not exists"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201250000001` | An item with the specified field values `paymentId` and `basketId` already exists ||
|| `201240400002` | Payment not found. Incorrect value for the provided parameter `paymentId` ||
|| `201240400003` | Basket item not found. Incorrect value for the provided parameter `basketId` ||
|| `200040300020` | Insufficient permissions to add a binding of the basket item to the payment ||
|| `100` | The parameter `fields` is not specified or is empty ||
|| `0` | Required fields are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-payment-item-basket-update.md)
- [{#T}](./sale-payment-item-basket-get.md)
- [{#T}](./sale-payment-item-basket-list.md)
- [{#T}](./sale-payment-item-basket-delete.md)
- [{#T}](./sale-payment-item-basket-get-fields.md)
