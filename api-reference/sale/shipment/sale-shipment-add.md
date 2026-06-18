# Add Shipment sale.shipment.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a shipment.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating a shipment ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **orderId***
[`sale_order.id`](../data-types.md) | Order identifier ||
|| **allowDelivery***
[`string`](../../data-types.md) | Delivery permission indicator
Possible values:
- `Y` – yes (delivery allowed)
- `N` – no (delivery not allowed) ||
|| **deducted***
[`string`](../../data-types.md) | Indicator of whether the shipment has been shipped
Possible values:
- `Y` – yes (shipped)
- `N` – no (not shipped) ||
|| **deliveryId***
[`sale_delivery_service.id`](../data-types.md) | Delivery service identifier ||
|| **statusId**
[`sale_status`](../data-types.md) | Delivery status identifier

If not provided, the status DN will be used (see the default status table in the documentation for [`sale.status.*`](../status/index.md)) ||
|| **deliveryDocDate**
[`datetime`](../../data-types.md) | Shipment document date ||
|| **deliveryDocNum**
[`string`](../../data-types.md) | Shipment document number ||
|| **trackingNumber**
[`string`](../../data-types.md) | Shipment identifier ||
|| **basePriceDelivery**
[`double`](../../data-types.md) | Base delivery cost (without discounts / surcharges).

If provided, the value will also be used as the delivery cost (shipment field `priceDelivery`).

If not provided, both the base delivery cost and the delivery cost will be calculated automatically based on the selected delivery service ||
|| **comments**
[`string`](../../data-types.md) | Manager's comment ||
|| **companyId**
[`integer`](../../data-types.md) | Company identifier from the "Online Store" module.

Currently not used ||
|| **responsibleId**
[`user`](../../data-types.md) | Identifier of the user responsible for the shipment ||
|| **xmlId**
[`string`](../../data-types.md) | External shipment identifier.

Can be used for synchronizing the shipment with an external system ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderId":2068,"allowDelivery":"Y","deducted":"Y","deliveryId":2,"statusId":"DN","deliveryDocDate":"2024-02-13T14:05:48","deliveryDocNum":"DocumentNumber123456","trackingNumber":"trackingNumber","basePriceDelivery":999.99,"comments":"My comment for manager","responsibleId":25,"xmlId":"myXmlId"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipment.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderId":2068,"allowDelivery":"Y","deducted":"Y","deliveryId":2,"statusId":"DN","deliveryDocDate":"2024-02-13T14:05:48","deliveryDocNum":"DocumentNumber123456","trackingNumber":"trackingNumber","basePriceDelivery":999.99,"comments":"My comment for manager","responsibleId":25,"xmlId":"myXmlId"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipment.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ShipmentAddResult = {
      shipment: {
        accountNumber: string
        allowDelivery: string
        basePriceDelivery: number
        canceled: string
        comments: string
        currency: string
        customPriceDelivery: string
        dateAllowDelivery: ISODate | null
        dateDeducted: ISODate | null
        dateInsert: ISODate | null
        dateResponsibleId: ISODate | null
        deducted: string
        deliveryDocDate: ISODate | null
        deliveryDocNum: string
        deliveryId: number
        deliveryName: string
        deliveryXmlId: string
        empAllowDeliveryId: number
        empDeductedId: number
        empResponsibleId: number
        id: number
        marked: string
        orderId: number
        priceDelivery: number
        responsibleId: number
        shipmentItems: unknown[]
        statusId: string
        statusXmlId: string
        system: string
        trackingNumber: string
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<ShipmentAddResult>({
        method: 'sale.shipment.add',
        params: {
          fields: {
            orderId: 2068,
            allowDelivery: 'Y',
            deducted: 'Y',
            deliveryId: 2,
            statusId: 'DN',
            deliveryDocDate: '2024-02-13T14:05:48',
            deliveryDocNum: 'DocumentNumber123456',
            trackingNumber: 'trackingNumber',
            basePriceDelivery: 999.99,
            comments: 'My comment for manager',
            responsibleId: 25,
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
        console.info('Added shipment id:', result.shipment.id, result.shipment)
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
      async function addShipment() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.shipment.add',
            params: {
              fields: {
                orderId: 2068,
                allowDelivery: 'Y',
                deducted: 'Y',
                deliveryId: 2,
                statusId: 'DN',
                deliveryDocDate: '2024-02-13T14:05:48',
                deliveryDocNum: 'DocumentNumber123456',
                trackingNumber: 'trackingNumber',
                basePriceDelivery: 999.99,
                comments: 'My comment for manager',
                responsibleId: 25,
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
          console.info('Added shipment id:', result.shipment.id, result.shipment)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addShipment)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.shipment.add',
                [
                    'fields' => [
                        'orderId'           => 2068,
                        'allowDelivery'     => 'Y',
                        'deducted'          => 'Y',
                        'deliveryId'        => 2,
                        'statusId'          => 'DN',
                        'deliveryDocDate'   => '2024-02-13T14:05:48',
                        'deliveryDocNum'    => 'DocumentNumber123456',
                        'trackingNumber'    => 'trackingNumber',
                        'basePriceDelivery' => 999.99,
                        'comments'          => 'My comment for manager',
                        'responsibleId'     => 25,
                        'xmlId'             => 'myXmlId',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding shipment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.shipment.add', {
            fields: {
                orderId: 2068,
                allowDelivery: 'Y',
                deducted: 'Y',
                deliveryId: 2,
                statusId: 'DN',
                deliveryDocDate: '2024-02-13T14:05:48',
                deliveryDocNum: 'DocumentNumber123456',
                trackingNumber: 'trackingNumber',
                basePriceDelivery: 999.99,
                comments: 'My comment for manager',
                responsibleId: 25,
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
        'sale.shipment.add',
        [
            'fields' => [
                'orderId' => 2068,
                'allowDelivery' => 'Y',
                'deducted' => 'Y',
                'deliveryId' => 2,
                'statusId' => 'DN',
                'deliveryDocDate' => '2024-02-13T14:05:48',
                'deliveryDocNum' => 'DocumentNumber123456',
                'trackingNumber' => 'trackingNumber',
                'basePriceDelivery' => 999.99,
                'comments' => 'My comment for manager',
                'responsibleId' => 25,
                'xmlId' => 'myXmlId',
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP status: **200**

```json
{
   "result":{
      "shipment":{
         "accountNumber":"2068\/19",
         "allowDelivery":"Y",
         "basePriceDelivery":999.99,
         "canceled":"N",
         "comments":"My comment for manager",
         "currency":"USD",
         "customPriceDelivery":"N",
         "dateAllowDelivery":"2024-04-11T14:17:52+02:00",
         "dateDeducted":"2024-04-11T14:17:52+02:00",
         "dateInsert":"2024-04-11T14:17:52+02:00",
         "dateResponsibleId":"2024-04-11T14:17:52+02:00",
         "deducted":"Y",
         "deliveryDocDate":"2024-02-13T13:05:48+02:00",
         "deliveryDocNum":"DocumentNumber123456",
         "deliveryId":2,
         "deliveryName":"Courier Delivery",
         "deliveryXmlId":"",
         "empAllowDeliveryId":1,
         "empDeductedId":1,
         "empResponsibleId":1,
         "id":2452,
         "marked":"N",
         "orderId":2068,
         "priceDelivery":999.99,
         "responsibleId":25,
         "shipmentItems":[
            
         ],
         "statusId":"DN",
         "statusXmlId":"FFdddd",
         "system":"N",
         "trackingNumber":"trackingNumber",
         "xmlId":"myXmlId"
      }
   },
   "time":{
      "start":1712837872.459187,
      "finish":1712837873.462857,
      "duration":1.0036699771881104,
      "processing":0.8182649612426758,
      "date_start":"2024-04-11T15:17:52+02:00",
      "date_finish":"2024-04-11T15:17:53+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **shipment**
[`sale_order_shipment`](../data-types.md) | Object containing information about the added shipment ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":0,
   "error_description":"Unable to load order"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | Insufficient permissions to add shipment ||
|| `100` | Parameter `fields` is not specified or is empty ||
|| `0` | Order not found ||
|| `SALE_SHIPMENT_WRONG_DELIVERY_SERVICE` | Delivery service not found ||
|| `BX_INVALID_VALUE` | Value of one of the fields did not pass validation before saving ||
|| `0` | Required fields not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-shipment-update.md)
- [{#T}](./sale-shipment-get.md)
- [{#T}](./sale-shipment-list.md)
- [{#T}](./sale-shipment-delete.md)
- [{#T}](./sale-shipment-get-fields.md)
