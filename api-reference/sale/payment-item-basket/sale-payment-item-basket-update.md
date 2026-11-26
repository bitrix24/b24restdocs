# Update the binding of the cart item to the payment sale.paymentitembasket.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.paymentitembasket.update` updates the binding of the cart item to the payment.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_payment_item_basket.id`](../data-types.md#sale_payment_item_basket) | Binding identifier. You can get the identifier using the method [sale.paymentitembasket.list](./sale-payment-item-basket-list.md) ||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the binding of the cart item to the payment ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **quantity***
[`double`](../../data-types.md) | Quantity of the product in the payment. Required in every call, even if only `xmlId` is changed. Must not exceed the quantity of the cart item.

The method does not check the sign or type: a negative value is saved, and a string is saved as `0` ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier of the binding ||
|#

The `paymentId` and `basketId` fields cannot be changed: the method silently ignores them. To move an item to another payment, delete the binding and create a new one.

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

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "quantity": 1,
        "xmlId": "myNewXmlId",
    }

    try:
        bitrix_response = client.sale.paymentitembasket.update(
            bitrix_id=1186,
            fields=fields,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sale.paymentitembasket.update", b24.Params{
    	"id": 1186,
    	"fields": b24.Params{
    		"quantity": 1,
    		"xmlId":    "myNewXmlId",
    	},
    })
    if err != nil {
    	return fmt.Errorf("sale.paymentitembasket.update: %w", err)
    }

    // The method wraps the response in an object with the "paymentItemBasket" key.
    raw, ok := b24.Unwrap(res.Result, "paymentItemBasket")
    if !ok {
    	return fmt.Errorf("no paymentItemBasket key in the response")
    }

    var item struct {
    	BasketID   b24.ID `json:"basketId"`
    	DateInsert string `json:"dateInsert"`
    	ID         b24.ID `json:"id"`
    	PaymentID  b24.ID `json:"paymentId"`
    	Quantity   int    `json:"quantity"`
    	XmlID      string `json:"xmlId"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.BasketID, item.DateInsert)
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
[`object`](../../data-types.md) | Root element of the response. Contains the `paymentItemBasket` key — the updated binding [(detailed description)](#paymentitembasket) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object paymentItemBasket {#paymentitembasket}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Binding identifier ||
|| **paymentId**
[`integer`](../../data-types.md) | Payment identifier ||
|| **basketId**
[`integer`](../../data-types.md) | Cart item identifier ||
|| **quantity**
[`double`](../../data-types.md) | Quantity of the product in the payment ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier of the binding ||
|| **dateInsert**
[`datetime`](../../data-types.md) | Binding creation date ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 201240400001,
    "error_description": "payment item is not exists"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `201240400001` | payment item is not exists | The binding with the specified `id` is not found ||
|| `400` | `0` | Required fields: quantity | The `quantity` field is not provided ||
|| `400` | `0` | Insufficient item quantity in shopping cart | `quantity` exceeds the quantity of the cart item ||
|| `400` | `100` | Could not find value for parameter {id} | The `id` parameter is not provided ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter is not provided ||
|| `400` | `200040300020` | Access Denied | No permission to modify data in the Online Store module ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-payment-item-basket-add.md)
- [{#T}](./sale-payment-item-basket-get.md)
- [{#T}](./sale-payment-item-basket-list.md)
- [{#T}](./sale-payment-item-basket-delete.md)
- [{#T}](./sale-payment-item-basket-get-fields.md)
