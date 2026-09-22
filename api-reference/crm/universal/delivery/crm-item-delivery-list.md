# Get the List of Deliveries for a CRM object crm.item.delivery.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with read access to the CRM object from which deliveries are selected

The method `crm.item.delivery.list` returns a list of deliveries for a specific CRM object. A delivery is a shipment of an order linked to a CRM object.

The method does not support pagination: all deliveries of the object arrive in a single response, the `start` parameter does not work, and the response contains no `next` and `total` fields.

Each list element contains the same brief set of fields that the [crm.item.delivery.get](./crm-item-delivery-get.md) method returns. The set is fixed — the method has no `select` parameter.

The list is empty if the object has no linked orders or if its type does not support deliveries. Which CRM objects support deliveries is described in the [overview of the section methods](./index.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityId***
[`integer`](../../../data-types.md) | Identifier of the CRM object whose deliveries you want to retrieve. For example, the identifier of a deal ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | Identifier of the [CRM object type](../../data-types.md#object_type). Only deals and invoices have deliveries: for them, `entityTypeId` equals `2` and `31` respectively ||
|| **filter**
[`object`](../../../data-types.md) | Filter that narrows the selection of the CRM object's deliveries [(detailed description)](#filter) ||
|| **order**
[`object`](../../../data-types.md) | Sorting order in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `order_N`:
- `asc` — ascending
- `desc` — descending

You can sort by the fields of the [sale_order_shipment](../../../sale/data-types.md#sale_order_shipment) object. By default, the list is sorted by `id` in ascending order ||
|#

### Parameter filter {#filter}

Filter keys are the fields of the [sale_order_shipment](../../../sale/data-types.md#sale_order_shipment) object written in camelCase. Most often, deliveries are selected by the fields that the method returns in the response — they are listed in the [description of the result array element](#result). You can also filter by other shipment fields, for example by `orderId`.

An additional prefix can be specified for the key to clarify the filter's behavior:

- `=` — equal to, also works with arrays
- `@` — the value is in the provided array
- `!=` — not equal to
- `>` — greater than
- `>=` — greater than or equal to
- `<` — less than
- `<=` — less than or equal to
- `%` — LIKE, substring search. The `%` character does not need to be passed in the value
- `!%` — NOT LIKE, substring search. The `%` character does not need to be passed in the value
- `=%` and `%=` — LIKE, the `%` character must be passed in the value, for example `"mol%"`
- `!=%` and `!%=` — NOT LIKE, the `%` character must be passed in the value

The method always adds its own conditions to the filter and ignores the passed values of these keys. With `=orderId`, it takes the shipments only of those orders that are linked to the CRM object. With `=system` and `!deliveryId`, it discards system shipments and shipments without a delivery service. The method applies the remaining conditions together with its own, and they can only narrow the selection.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityId":13127,"entityTypeId":2,"filter":{"@id":[4077,4078]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.item.delivery.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityId":13127,"entityTypeId":2,"filter":{"@id":[4077,4078]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.item.delivery.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each DeliveryItem returned in result[]
    type DeliveryItem = {
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
      // crm.item.delivery.list has no pagination: it returns every delivery of the CRM item
      // in one response, so the list helpers (callList/fetchList) are not needed here.
      const response = await $b24.actions.v2.call.make<DeliveryItem[]>({
        method: 'crm.item.delivery.list',
        params: {
          entityId: 13127,
          entityTypeId: 2,
          filter: {
            '@id': [4077, 4078],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Deliveries fetched:', result.length, result)
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
      async function listDeliveries() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.item.delivery.list has no pagination: it returns every delivery of the CRM item
          // in one response, so the list helpers (callList/fetchList) are not needed here.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.item.delivery.list',
            params: {
              entityId: 13127,
              entityTypeId: 2,
              filter: {
                '@id': [4077, 4078],
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
          console.info('Deliveries fetched:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listDeliveries)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.item.delivery.list(
            entity_id=13127,
            entity_type_id=2,
            filter={
                "@id": [4077, 4078],
            },
        ).response
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
                'crm.item.delivery.list',
                [
                    'entityId'     => 13127,
                    'entityTypeId' => 2,
                    'filter'       => [
                        '@id' => [4077, 4078]
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching delivery list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.item.delivery.list', {
            entityId: 13127,
            entityTypeId: 2,
            filter: {
                "@id": [4077, 4078]
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
        'crm.item.delivery.list',
        [
            'entityId' => 13127,
            'entityTypeId' => 2,
            'filter' => [
                "@id" => [4077, 4078]
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
    res, err := client.Core().Call(ctx, "crm.item.delivery.list", b24.Params{
    	"entityId":     13127,
    	"entityTypeId": 2,
    	"filter": b24.Params{
    		"@id": []int{4077, 4078},
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.item.delivery.list: %w", err)
    }

    var items []struct {
    	ID            b24.ID  `json:"id"`
    	AccountNumber string  `json:"accountNumber"`
    	Deducted      string  `json:"deducted"`
    	DateDeducted  *string `json:"dateDeducted"`
    	DeliveryID    b24.ID  `json:"deliveryId"`
    	PriceDelivery float64 `json:"priceDelivery"`
    	Currency      string  `json:"currency"`
    	DeliveryName  string  `json:"deliveryName"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    // The method returns all deliveries of the object at once, so res.Total and res.Next
    // are not filled in and there is no need to traverse pages with client.Core().Pages.
    for _, it := range items {
    	fmt.Println(it.ID, it.AccountNumber, it.DeliveryName)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
   "result":[
      {
         "id":4077,
         "accountNumber":"3657\/2",
         "deducted":"N",
         "dateDeducted":null,
         "deliveryId":228,
         "priceDelivery":79.99,
         "currency":"USD",
         "deliveryName":"Uber Taxi (Cargo)"
      },
      {
         "id":4078,
         "accountNumber":"3657\/3",
         "deducted":"N",
         "dateDeducted":null,
         "deliveryId":228,
         "priceDelivery":79.99,
         "currency":"USD",
         "deliveryName":"Uber Taxi (Cargo)"
      }
   ],
   "time":{
      "start":1716369036.246855,
      "finish":1716369036.734466,
      "duration":0.4876110553741455,
      "processing":0.18442106246948242,
      "date_start":"2024-05-22T12:10:36+03:00",
      "date_finish":"2024-05-22T12:10:36+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md) | Array of objects with brief information about the selected deliveries [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Element of the result Array {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`sale_order_shipment.id`](../../../sale/data-types.md#sale_order_shipment) | Delivery identifier. The [crm.item.delivery.get](./crm-item-delivery-get.md) method works with it ||
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
   "error":"100",
   "error_description":"Could not find value for parameter {entityId}"
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {entityId} | The required parameter `entityId` is not provided ||
|| `400` | `100` | Could not find value for parameter {entityTypeId} | The required parameter `entityTypeId` is not provided ||
|| `400` | `100` | Invalid value {value} to match with parameter {entityId}. Should be value of type int | The value of `entityId` or `entityTypeId` cannot be cast to an integer. The error text names the specific parameter ||
|| `400` | `100` | Invalid value {value} to match with parameter {filter}. Should be value of type array | The value of `filter` or `order` is not passed as an object. The error text names the specific parameter ||
|| `400` | `ACCESS_DENIED` | Access denied | The user has no read access to the CRM object whose deliveries are requested. The method returns the same error if the pair of `entityId` and `entityTypeId` does not point to a CRM object — for example, when `entityId` equals `0` ||
|#

{% include notitle [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-item-delivery-get.md)
- [{#T}](./index.md)
- [{#T}](../payment/delivery-in-payment/index.md)
- [{#T}](../../../sale/shipment/sale-shipment-list.md)