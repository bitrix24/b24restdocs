# Add Shipment sale.shipment.add

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

Relevant only for Bitrix Site Management. Not related to CRM companies ||
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

- JS

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

- PHP

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
         "dateAllowDelivery":"2024-04-11T14:17:52+03:00",
         "dateDeducted":"2024-04-11T14:17:52+03:00",
         "dateInsert":"2024-04-11T14:17:52+03:00",
         "dateResponsibleId":"2024-04-11T14:17:52+03:00",
         "deducted":"Y",
         "deliveryDocDate":"2024-02-13T13:05:48+03:00",
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
      "date_start":"2024-04-11T15:17:52+03:00",
      "date_finish":"2024-04-11T15:17:53+03:00"
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
[`sale_order_shipment`](../data-types.md) | Object with information about the added shipment ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":0,
   "error_description":"Unable to load the order"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | Insufficient permissions to add shipment ||
|| `100` | Parameter `fields` is missing or empty ||
|| `0` | Order not found ||
|| `SALE_SHIPMENT_WRONG_DELIVERY_SERVICE` | Delivery service not found ||
|| `BX_INVALID_VALUE` | Value of one of the fields failed validation before saving ||
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