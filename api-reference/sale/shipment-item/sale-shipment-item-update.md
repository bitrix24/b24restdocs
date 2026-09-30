# Update an item in the shipment table part sale.shipmentitem.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.shipmentitem.update` changes the product quantity and the external identifier of a shipment table part item.

The basket item and the shipment of an item cannot be changed: the method ignores the passed `basketId` and `orderDeliveryId` without an error. To move a product to another shipment, delete the item using the [sale.shipmentitem.delete](./sale-shipment-item-delete.md) method and add a new one using the [sale.shipmentitem.add](./sale-shipment-item-add.md) method.

Items of the system shipment cannot be changed. In a shipment with `deducted` = `Y`, only `xmlId` can be changed, and `quantity` must contain the current value. For the rules on distributing quantity across shipments, see the [section overview](./index.md#quantity).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_shipment_item.id`](../data-types.md#sale_order_shipment_item) | Identifier of the shipment table part item.

Can be retrieved using the [sale.shipmentitem.list](./sale-shipment-item-list.md) method ||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the shipment table part item [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **quantity***
[`double`](../../data-types.md) | Quantity of the product in the shipment. Must be greater than `0`.

The total quantity across all shipments of the order cannot exceed the quantity in the basket item ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier. If the field is omitted, the previous value is retained.

Can be used to synchronize the shipment table part item with a similar position in an external system ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7,"fields":{"quantity":5,"xmlId":"myNewXmlId"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipmentitem.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7,"fields":{"quantity":5,"xmlId":"myNewXmlId"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipmentitem.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ShipmentItemUpdateResult = {
      shipmentItem: {
        basketId: number
        dateInsert: ISODate
        id: number
        orderDeliveryId: number
        quantity: number
        reservedQuantity: number
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<ShipmentItemUpdateResult>({
        method: 'sale.shipmentitem.update',
        params: {
          id: 7,
          fields: {
            quantity: 5,
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
        console.info(result.shipmentItem)
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
      async function updateShipmentItem() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.shipmentitem.update',
            params: {
              id: 7,
              fields: {
                quantity: 5,
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
          console.info(result.shipmentItem)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateShipmentItem)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "quantity": 5,
        "xmlId": "myNewXmlId",
    }

    try:
        bitrix_response = client.sale.shipmentitem.update(
            bitrix_id=7,
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
                'sale.shipmentitem.update',
                [
                    'id' => 7,
                    'fields' => [
                        'quantity' => 5,
                        'xmlId' => 'myNewXmlId',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating shipment item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
       'sale.shipmentitem.update', {
            id: 7,
            fields: {
                quantity: 5,
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
        'sale.shipmentitem.update',
        [
            'id' => 7,
            'fields' => [
                'quantity' => 5,
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
    res, err := client.Core().Call(ctx, "sale.shipmentitem.update", b24.Params{
    	"id": 7,
    	"fields": b24.Params{
    		"quantity": 5,
    		"xmlId":    "myNewXmlId",
    	},
    })
    if err != nil {
    	return fmt.Errorf("sale.shipmentitem.update: %w", err)
    }

    // The method wraps the response in an object with the "shipmentItem" key.
    raw, ok := b24.Unwrap(res.Result, "shipmentItem")
    if !ok {
    	return fmt.Errorf("no shipmentItem key in the response")
    }

    var item struct {
    	BasketID         b24.ID `json:"basketId"`
    	DateInsert       string `json:"dateInsert"`
    	ID               b24.ID `json:"id"`
    	OrderDeliveryID  b24.ID `json:"orderDeliveryId"`
    	Quantity         float64 `json:"quantity"`
    	ReservedQuantity float64 `json:"reservedQuantity"`
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
        "shipmentItem": {
            "basketId": 2716,
            "dateInsert": "2024-04-11T09:10:34+02:00",
            "id": 7,
            "orderDeliveryId": 2431,
            "quantity": 5,
            "reservedQuantity": 0,
            "xmlId": "myNewXmlId"
        }
    },
    "time": {
        "start": 1712819636.302217,
        "finish": 1712819637.183715,
        "duration": 0.8814980983734131,
        "processing": 0.6984810829162598,
        "date_start": "2024-04-11T10:13:56+02:00",
        "date_finish": "2024-04-11T10:13:57+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **shipmentItem**
[`sale_order_shipment_item`](../data-types.md#sale_order_shipment_item) | The updated shipment table part item ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "100",
    "error_description": "Could not find value for parameter {fields}"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201240400001` | `shipment item is not exists`

The updated shipment table part item was not found ||
|| `200040300020` | `Access Denied`

Insufficient rights to update the shipment table part item ||
|| `100` | `Bitrix\Sale\ShipmentItem constructor must be is public`

The `id` parameter is not specified ||
|| `100` | `Could not find value for parameter {fields}`

The `fields` parameter is not specified ||
|| `0` | `Required fields: quantity`

The `quantity` field is not provided in `fields` ||
|| `150` | `System shipment cannot be modified`

The item belongs to the system shipment, which holds the unallocated product ||
|| `SALE_SHIPMENT_ITEM_SHIPMENT_ALREADY_SHIPPED_CANNOT_EDIT` | `Shipment already done. No change is possible.`

The shipment is already shipped (`deducted` = `Y`), and `quantity` differs from the current value ||
|| `SALE_SHIPMENT_ITEM_LESS_AVAILABLE_QUANTITY` | `The available quantity of "<name>" is less than that specified in the shopping cart. Check if this product has been added to the order's other shipments.`

The basket does not have enough unallocated product: the quantity exceeds the remainder not distributed to other shipments ||
|| `SALE_SHIPMENT_ITEM_ERR_QUANTITY_EMPTY` | `The quantity of <name> cannot be zero or less than zero`

`quantity` = `0` is passed ||
|| `BARCODE_MORE_ITEM_QUANTITY` | `The number of barcodes is more than product quantity`

A negative `quantity` value is passed ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-shipment-item-add.md)
- [{#T}](./sale-shipment-item-get.md)
- [{#T}](./sale-shipment-item-list.md)
- [{#T}](./sale-shipment-item-delete.md)
- [{#T}](./sale-shipment-item-get-fields.md)
