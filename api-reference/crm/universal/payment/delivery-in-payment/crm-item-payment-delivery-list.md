# Get Delivery Item List by Specific Payment crm.item.payment.delivery.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to read the payment

The method `crm.item.payment.delivery.list` returns all delivery items of a payment in a single response, without pagination.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **paymentId***
[`sale_order_payment.id`](../../../../sale/data-types.md#sale_order_payment) | Identifier of the payment. You can retrieve the payment identifiers of a CRM object using the [crm.item.payment.list](../crm-item-payment-list.md) method ||
|| **filter**
[`object`](../../../../data-types.md) | Filter in the format `{"field": "value"}`. Available fields:
- `id` — identifier of the delivery item in the payment
- `quantity` — quantity
- `entityId` — identifier of the delivery document. In the response, this field is called `deliveryId`, but in the filter you must use `entityId`

You can add an operator prefix to the key:
- `=` — equals, used by default
- `!=` — not equal
- `>`, `>=`, `<`, `<=` — greater than, greater than or equal to, less than, less than or equal to
- `@` — in an array of values
- `!@` — not in an array of values

All filter fields are numeric, so the substring search operators `%`, `=%`, `%=`, and their negations do not apply: the method accepts them without an error but finds no matches.

If you pass another field, for example `deliveryId`, the method returns the `Internal server error.` error. By default, no filter is applied. In the Python SDK, `filter` is a required method argument: if you do not need a filter, pass `{}` ||
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
    -d '{"paymentId":1040,"filter":{"@id":[1201], ">=quantity":1}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.payment.delivery.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"paymentId":1040,"filter":{"@id":[1201], ">=quantity":1},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.payment.delivery.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each item returned in result[]
    type CrmItemPaymentDelivery = {
      id: number
      paymentId: number
      quantity: number
      deliveryId: number
    }

    try {
      // crm.item.payment.delivery.list returns all records in one response, without pagination
      const response = await $b24.actions.v2.call.make<CrmItemPaymentDelivery[]>({
        method: 'crm.item.payment.delivery.list',
        params: {
          paymentId: 1040,
          filter: {
            '>=quantity': 1,
            '@id': [1201],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Delivery items count:', result.length, result)
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
      async function listPaymentDeliveries() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.item.payment.delivery.list returns all records in one response, without pagination
          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.payment.delivery.list',
            params: {
              paymentId: 1040,
              filter: {
                '>=quantity': 1,
                '@id': [1201],
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
          console.info('Delivery items count:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listPaymentDeliveries)
    </script>
    ```

- Python

    Example

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.item.payment.delivery.list(
            payment_id=1040,
            filter={
                "@id": [1201],
                ">=quantity": 1,
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
        bitrix_response = client.crm.item.payment.delivery.list(
            payment_id=1040,
            filter={
                "@id": [1201],
                ">=quantity": 1,
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
                'crm.item.payment.delivery.list',
                [
                    'paymentId' => 1040,
                    'filter'    => [
                        ">=quantity" => 1,
                        "@id"        => [1201],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching payment delivery list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.payment.delivery.list', {
            paymentId: 1040,
            filter: {
                ">=quantity": 1,
                "@id": [1201],
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
        'crm.item.payment.delivery.list',
        [
            'paymentId' => 1040,
            'filter' => [
                '>=quantity' => 1,
                '@id' => [1201],
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
    res, err := client.Core().Call(ctx, "crm.item.payment.delivery.list", b24.Params{
    	"paymentId": 1040,
    	"filter": b24.Params{
    		"@id":        []int{1201},
    		">=quantity": 1,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.item.payment.delivery.list: %w", err)
    }

    var items []struct {
    	ID         b24.ID `json:"id"`
    	PaymentID  b24.ID `json:"paymentId"`
    	Quantity   int    `json:"quantity"`
    	DeliveryID b24.ID `json:"deliveryId"`
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
         "id":1201,
         "paymentId":1040,
         "quantity":1,
         "deliveryId":4073
      }
   ],
   "time":{
      "start":1716301848.792584,
      "finish":1716301849.095721,
      "duration":0.30313706398010254,
      "processing":0.05563783645629883,
      "date_start":"2024-05-21T17:30:48+03:00",
      "date_finish":"2024-05-21T17:30:49+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`crm_item_payment_delivery[]`](#crm_item_payment_delivery) | Array of delivery items of the payment [(detailed description)](#crm_item_payment_delivery). If there are no items, an empty array is returned ||
|| **time**
[`time`](../../../../data-types.md) | Information about the request execution time ||
|#

#### crm_item_payment_delivery {#crm_item_payment_delivery}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the delivery item in the payment ||
|| **paymentId**
[`sale_order_payment.id`](../../../../sale/data-types.md#sale_order_payment) | Payment identifier  ||
|| **quantity**
[`double`](../../../../data-types.md) | Quantity. For a delivery item, always `1` ||
|| **deliveryId**
[`sale_order_shipment.id`](../../../../sale/data-types.md#sale_order_shipment) | Identifier of the delivery document ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "100",
    "error_description": "Internal server error."
}
```

{% include notitle [Error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Internal server error. | An unsupported field was passed in `filter` or `order`, for example `deliveryId` instead of `entityId` ||
|| `400` | `0` | Payment has not been found | The payment with the passed `paymentId` was not found ||
|| `400` | `ACCESS_DENIED` | Access denied | The user does not have permission to read the payment ||
|| `400` | `100` | Could not find value for parameter {paymentId} | The required parameter `paymentId` was not provided ||
|#

The method returns code `0` for several different errors. To determine the cause, check the text in `error_description`.

{% include [System errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-payment-delivery-add.md)
- [{#T}](./crm-item-payment-delivery-delete.md)
- [{#T}](./crm-item-payment-delivery-set-delivery.md)