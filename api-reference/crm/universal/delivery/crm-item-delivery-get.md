# Get Delivery Information by ID crm.item.delivery.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with read access to the order the delivery belongs to

The method `crm.item.delivery.get` returns brief information about a delivery.

A delivery is a shipment of an order linked to a CRM object. The method returns a fixed set of fields — you cannot select other shipment fields. The full set is returned by the [sale.shipment.get](../../../sale/shipment/sale-shipment-get.md) method, but it is available to administrators only.

How deliveries are linked to CRM objects is described in the [overview of the section methods](./index.md).

The method does not check which CRM object the delivery belongs to and returns any delivery the user has access to. The `crm.item.delivery.list` method excludes system shipments and shipments without a delivery service from the list, while `crm.item.delivery.get` returns them.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_shipment.id`](../../../sale/data-types.md#sale_order_shipment) | Delivery identifier.

You can retrieve the delivery identifiers of a CRM object using the [crm.item.delivery.list](./crm-item-delivery-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":4077}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.delivery.get
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":4077,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.delivery.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DeliveryGetResult = {
      id: number
      accountNumber: string
      deducted: string
      dateDeducted: ISODate | null
      deliveryId: number
      priceDelivery: number
      currency: string
      deliveryName: string
    }

    try {
      const response = await $b24.actions.v2.call.make<DeliveryGetResult>({
        method: 'crm.item.delivery.get',
        params: {
          id: 4077,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.id, result.deliveryName, result.priceDelivery, result.currency)
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
      async function getDelivery() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.delivery.get',
            params: {
              id: 4077,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.id, result.deliveryName, result.priceDelivery, result.currency)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getDelivery)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.item.delivery.get(
            bitrix_id=4077,
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
                'crm.item.delivery.get',
                [
                    'id' => 4077,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting delivery item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.delivery.get', {
            id: 4077,
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
        'crm.item.delivery.get',
        [
            'id' => 4077
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.item.delivery.get", b24.Params{
    	"id": 4077,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.item.delivery.get: %w", err)
    }

    var item struct {
    	ID            b24.ID  `json:"id"`
    	AccountNumber string  `json:"accountNumber"`
    	Deducted      string  `json:"deducted"`
    	DateDeducted  *string `json:"dateDeducted"`
    	DeliveryID    b24.ID  `json:"deliveryId"`
    	PriceDelivery float64 `json:"priceDelivery"`
    	Currency      string  `json:"currency"`
    	DeliveryName  string  `json:"deliveryName"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.AccountNumber, item.DeliveryName)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
   "result":{
      "id":4077,
      "accountNumber":"3657\/2",
      "deducted":"N",
      "dateDeducted":null,
      "deliveryId":228,
      "priceDelivery":79.99,
      "currency":"USD",
      "deliveryName":"Uber Taxi (Cargo)"
   },
   "time":{
      "start":1716369295.614557,
      "finish":1716369296.143089,
      "duration":0.5285320281982422,
      "processing":0.2371680736541748,
      "date_start":"2024-05-22T12:14:55+03:00",
      "date_finish":"2024-05-22T12:14:56+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with brief information about the delivery [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`sale_order_shipment.id`](../../../sale/data-types.md#sale_order_shipment) | Delivery identifier ||
|| **accountNumber**
[`string`](../../../data-types.md) | System delivery number. For example, `3657/2` ||
|| **deducted**
[`string`](../../../data-types.md) | Indicates whether the delivery has been shipped.

Possible values:
- `Y` — shipped
- `N` — not shipped ||
|| **dateDeducted**
[`datetime`](../../../data-types.md) | Date and time when the `deducted` flag was last changed. The field returns `null` if the flag has never been changed ||
|| **deliveryId**
[`sale_delivery_service.id`](../../../sale/data-types.md#sale_delivery_service) | Delivery service identifier. You can retrieve the list of delivery services using the [sale.delivery.getlist](../../../sale/delivery/delivery/sale-delivery-get-list.md) method ||
|| **priceDelivery**
[`double`](../../../data-types.md) | Delivery cost ||
|| **currency**
[`string`](../../../data-types.md) | Character code of the delivery currency. For example, `USD` ||
|| **deliveryName**
[`string`](../../../data-types.md) | Delivery service name. For example, `Uber Taxi (Cargo)` ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":"0",
   "error_description":"Delivery has not been found"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Delivery has not been found | There is no delivery with such `id` in Bitrix24 ||
|| `400` | `100` | Could not find value for parameter {id} | The required parameter `id` is not provided ||
|| `400` | `100` | Invalid value {value} to match with parameter {id}. Should be value of type int | The value of `id` cannot be cast to an integer ||
|| `400` | `ACCESS_DENIED` | Access denied | The user has no read access to the order the delivery belongs to. Read access to the CRM object where this delivery is visible does not clear the error ||
|#

{% include notitle [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-delivery-list.md)
- [{#T}](./index.md)
- [{#T}](../payment/delivery-in-payment/index.md)
- [{#T}](../../../sale/delivery/delivery/sale-delivery-get-list.md)