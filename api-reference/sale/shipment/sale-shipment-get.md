# Get Shipment Fields sale.shipment.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves the values of all shipment fields.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_shipment.id`](../data-types.md) | Shipment identifier ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":2465}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipment.get
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":2465,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipment.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ShipmentGetResult = {
      shipment: {
        accountNumber: string
        allowDelivery: string
        basePriceDelivery: number
        canceled: string
        comments: string
        companyId: number
        currency: string
        customPriceDelivery: string
        dateAllowDelivery: ISODate | null
        dateCanceled: ISODate | null
        dateDeducted: ISODate | null
        dateInsert: ISODate
        dateMarked: ISODate | null
        dateResponsibleId: ISODate
        deducted: string
        deliveryDocDate: ISODate | null
        deliveryDocNum: string
        deliveryId: number
        deliveryName: string
        deliveryXmlId: string
        discountPrice: number
        empAllowDeliveryId: number | null
        empCanceledId: number | null
        empDeductedId: number | null
        empMarkedId: number | null
        empResponsibleId: number | null
        externalDelivery: string
        id: number
        id1c: string
        marked: string
        orderId: number
        priceDelivery: number
        reasonMarked: string
        reasonUndoDeducted: string
        responsibleId: number
        shipmentItems: Array<{
          basketId: number
          dateInsert: ISODate
          id: number
          orderDeliveryId: number
          quantity: number
          reservedQuantity: number
          xmlId: string
        }>
        statusId: string
        statusXmlId: string
        system: string
        trackingDescription: string
        trackingLastCheck: string
        trackingNumber: string
        trackingStatus: string
        updated1c: string
        version1c: string
        xmlId: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<ShipmentGetResult>({
        method: 'sale.shipment.get',
        params: {
          id: 2465,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.shipment.id, result.shipment.accountNumber, result.shipment.statusId)
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
      async function getShipment() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sale.shipment.get',
            params: {
              id: 2465,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.shipment.id, result.shipment.accountNumber, result.shipment.statusId)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getShipment)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.shipment.get',
                [
                    'id' => 2465,
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
        echo 'Error getting shipment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.shipment.get", {
            "id": 2465
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
        'sale.shipment.get',
        [
            'id' => 2465
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
   "result": {
      "shipment": {
         "accountNumber": "2069/2",
         "allowDelivery": "N",
         "basePriceDelivery": 600,
         "canceled": "N",
         "comments": "",
         "companyId": 0,
         "currency": "USD",
         "customPriceDelivery": "N",
         "dateAllowDelivery": null,
         "dateCanceled": null,
         "dateDeducted": null,
         "dateInsert": "2024-04-11T15:05:48+02:00",
         "dateMarked": null,
         "dateResponsibleId": "2024-04-11T15:05:48+02:00",
         "deducted": "N",
         "deliveryDocDate": null,
         "deliveryDocNum": "",
         "deliveryId": 2,
         "deliveryName": "Courier Delivery",
         "deliveryXmlId": "",
         "discountPrice": 0,
         "empAllowDeliveryId": null,
         "empCanceledId": null,
         "empDeductedId": null,
         "empMarkedId": null,
         "empResponsibleId": null,
         "externalDelivery": "N",
         "id": 2465,
         "id1c": "",
         "marked": "N",
         "orderId": 2069,
         "priceDelivery": 600,
         "reasonMarked": "",
         "reasonUndoDeducted": "",
         "responsibleId": 0,
         "shipmentItems": [
            {
               "basketId": 2721,
               "dateInsert": "2024-04-11T15:05:51+02:00",
               "id": 10,
               "orderDeliveryId": 2465,
               "quantity": 1,
               "reservedQuantity": 1,
               "xmlId": "bx_6617e02cb74f9"
            }
         ],
         "statusId": "DN",
         "statusXmlId": "FFdddd",
         "system": "N",
         "trackingDescription": "",
         "trackingLastCheck": "",
         "trackingNumber": "",
         "trackingStatus": "",
         "updated1c": "N",
         "version1c": "",
         "xmlId": "bx_6617e02cae2a1"
      }
   },
   "time": {
      "start": 1712840827.02634,
      "finish": 1712840827.41618,
      "duration": 0.38983988761901855,
      "processing": 0.21664810180664062,
      "date_start": "2024-04-11T16:07:07+02:00",
      "date_finish": "2024-04-11T16:07:07+02:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **property**
[`sale_order_shipment`](../data-types.md) | Shipment information ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error": 201140400001,
   "error_description": "shipment does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201140400001` | Shipment not found ||
|| `200040300010` | Insufficient permissions to read the shipment ||
|| `100` | Parameter `id` not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-shipment-add.md)
- [{#T}](./sale-shipment-update.md)
- [{#T}](./sale-shipment-list.md)
- [{#T}](./sale-shipment-delete.md)
- [{#T}](./sale-shipment-get-fields.md)
