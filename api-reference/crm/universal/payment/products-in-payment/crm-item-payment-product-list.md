# Get a List of Payment Product Items crm.item.payment.product.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with "read" access permission for the CRM object associated with the payment

The method `crm.item.payment.product.list` returns all product items of a payment in a single response, without pagination.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **paymentId***
[`sale_order_payment.id`](../../../../sale/data-types.md#sale_order_payment) | Identifier of the payment. You can retrieve the payment identifiers of a CRM object using the [crm.item.payment.list](../crm-item-payment-list.md) method ||
|| **filter**
[`object`](../../../../data-types.md) | Filter in the format `{"field": "value"}`. Available fields:
- `id` — identifier of the product item in the payment
- `quantity` — quantity
- `entityId` — identifier of the product in the order basket

You can add an operator prefix to the key:
- `=` — equals, used by default
- `!=` — not equal
- `>`, `>=`, `<`, `<=` — greater than, greater than or equal to, less than, less than or equal to
- `@` — in an array of values
- `!@` — not in an array of values

All filter fields are numeric, so the substring search operators `%`, `=%`, `%=`, and their negations do not apply: the method accepts them without an error but finds no matches.

If you pass another field, the method returns the `Internal server error.` error. By default, no filter is applied. In the Python SDK, `filter` is a required method argument: if you do not need a filter, pass `{}` ||
|| **order**
[`object`](../../../../data-types.md) | Sort order in the format `{"field": "direction"}`. The fields are the same as in `filter`. Direction:
- `ASC` — ascending
- `DESC` — descending

Defaults to `{"id": "ASC"}` ||
|#

The method does not support the `start` parameter: all payment items are returned in a single response.

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"paymentId":1039,"filter":{"\u003E=quantity":2,"@id":[1195,1196]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.payment.product.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"paymentId":1039,"filter":{"\u003E=quantity":2,"@id":[1195,1196]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.payment.product.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each item returned in result[]
    type PaymentProductItem = {
      id: number,
      paymentId: number,
      quantity: number,
      entityId?: number,
      rowId?: number,
    }

    try {
      const response = await $b24.actions.v2.call.make<PaymentProductItem[]>({
        method: 'crm.item.payment.product.list',
        params: {
          paymentId: 1039,
          filter: {
            '>=quantity': 2,
            '@id': [1195, 1196],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Payment products:', result.length, result)
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
      async function fetchPaymentProductList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.payment.product.list',
            params: {
              paymentId: 1039,
              filter: {
                '>=quantity': 2,
                '@id': [1195, 1196],
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
          console.info('Payment products:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchPaymentProductList)
    </script>
    ```

- Python

    Example

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.item.payment.product.list(
            payment_id=1039,
            filter={
                ">=quantity": 2,
                "@id": [1195, 1196],
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

    Example `as_list`

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.item.payment.product.list(
            payment_id=1039,
            filter={
                ">=quantity": 2,
                "@id": [1195, 1196],
            },
        ).as_list().response
        result = bitrix_response.result
        for item in result:
            print(item)
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
                'crm.item.payment.product.list',
                [
                    'paymentId' => 1039,
                    'filter'    => [
                        '>=quantity' => 2,
                        '@id'        => [1195, 1196],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching payment product list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.payment.product.list', {
            paymentId: 1039,
            filter: {
                ">=quantity": 2,
                "@id": [1195, 1196],
            },
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
        'crm.item.payment.product.list',
        [
            'paymentId' => 1039,
            'filter' => [
                ">=quantity" => 2,
                "@id" => [1195, 1196],
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
    res, err := client.Core().Call(ctx, "crm.item.payment.product.list", b24.Params{
    	"paymentId": 1039,
    	"filter": b24.Params{
    		">=quantity": 2,
    		"@id":        []int{1195, 1196},
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.item.payment.product.list: %w", err)
    }

    var items []struct {
        ID        b24.ID  `json:"id"`
        PaymentID b24.ID  `json:"paymentId"`
        Quantity  int     `json:"quantity"`
        EntityID  *b24.ID `json:"entityId"`
        RowID     *b24.ID `json:"rowId"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.PaymentID)
    }

    // Total and Next are filled in by list methods; for a full
    // list traversal, use client.Core().Pages and Scan.
    if res.Total != nil {
    	fmt.Println("total:", *res.Total)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
   "result":[
      {
         "id":433,
         "paymentId":553,
         "quantity":2,
         "entityId":1349
      }
   ],
   "time":{
      "start":1790581835,
      "finish":1790581835.956596,
      "duration":0.9565958976745605,
      "processing":0,
      "date_start":"2026-09-28T10:50:35+03:00",
      "date_finish":"2026-09-28T10:50:35+03:00",
      "operating_reset_at":1790582435,
      "operating":0
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`crm_item_payment_product[]`](#crm_item_payment_product) | Array of product items of the payment [(detailed description)](#crm_item_payment_product). If there are no items, an empty array is returned ||
|| **time**
[`time`](../../../../data-types.md) | Information about the request execution time ||
|#

#### crm_item_payment_product {#crm_item_payment_product}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the product item in the payment ||
|| **paymentId**
[`sale_order_payment.id`](../../../../sale/data-types.md#sale_order_payment) | Payment identifier ||
|| **quantity**
[`double`](../../../../data-types.md) | Quantity of the product ||
|| **entityId**
[`integer`](../../../../data-types.md) | Identifier of the order product item. Returned if the payment item is not matched to a product row in the CRM object ||
|| **rowId**
[`integer`](../../../../data-types.md) | Identifier of the product row in the CRM object. Returned if the payment item is matched to a product row in the CRM object ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Payment has not been found"
}
```

{% include notitle [Error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Payment has not been found | The payment with the passed `paymentId` was not found ||
|| `400` | `ACCESS_DENIED` | Access denied | The user does not have permission to read the payment ||
|| `400` | `100` | Internal server error. | An unsupported field was passed in `filter` or `order` ||
|| `400` | `100` | Could not find value for parameter {paymentId} | The required parameter `paymentId` was not provided ||
|#

The method returns code `0` for several different errors. To determine the cause, check the text in `error_description`.

{% include [System errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-payment-product-add.md)
- [{#T}](./crm-item-payment-product-delete.md)
- [{#T}](./crm-item-payment-product-set-quantity.md)
