# Update Shipment sale.shipment.update

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates a shipment.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_shipment.id`](../data-types.md) | Identifier of the shipment ||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the shipment ||
|#

### Parameter fields

General parameters relevant for shipment properties of any type:

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **allowDelivery***
[`string`](../../data-types.md) | Indicator of delivery permission.
Possible values:
- `Y` – yes (delivery allowed)
- `N` – no (delivery not allowed) ||
|| **deducted***
[`string`](../../data-types.md) | Indicator of whether the shipment has been deducted.
Possible values:
- `Y` – yes (deducted)
- `N` – no (not deducted) ||
|| **deliveryId***
[`sale_delivery_service`](../data-types.md) | Identifier of the delivery service ||
|| **statusId**
[`sale_status`](../../data-types.md) | Identifier of the delivery status.

If not provided, the status DN is used (see the default status table in the documentation for [`sale.status.*`](../status/index.md)) ||
|| **deliveryDocDate**
[`datetime`](../../data-types.md) | Date of the shipment document ||
|| **deliveryDocNum**
[`string`](../../data-types.md) | Number of the shipment document ||
|| **trackingNumber**
[`string`](../../data-types.md) | Identifier of the shipment ||
|| **basePriceDelivery**
[`double`](../../data-types.md) | Base delivery cost (without discounts / surcharges).

If provided, it is also used for setting the value of `priceDelivery`. The provided value of `priceDelivery` is ignored in this case.

If neither basePriceDelivery nor priceDelivery is provided, both prices are set to 0 ||
|| **priceDelivery**
[`double`](../../data-types.md) | Delivery cost.

If provided and `basePriceDelivery` is not set, it is used for setting the value of `basePriceDelivery`.

If neither `basePriceDelivery` nor `priceDelivery` is provided, both prices are set to 0 ||
|| **comments**
[`string`](../../data-types.md) | Manager's comment ||
|| **companyId**
[`integer`](../../data-types.md) | Identifier of the company from the "Online Store" module.

Relevant only for Bitrix Site Management. Not related to CRM companies ||
|| **responsibleId**
[`user`](../../data-types.md) | Identifier of the user responsible for the shipment ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier of the shipment.

Can be used for synchronizing the shipment with an external system ||
|#
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":2452,"fields":{"allowDelivery":"N","deducted":"N","deliveryId":3,"statusId":"DD","deliveryDocDate":"2024-02-13T15:05:49","deliveryDocNum":"MyDocumentNumber","trackingNumber":"MyTrackingNumber","basePriceDelivery":1999.99,"comments":"My new comment for manager","responsibleId":1,"xmlId":"myNewXmlId"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipment.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":2452,"fields":{"allowDelivery":"N","deducted":"N","deliveryId":3,"statusId":"DD","deliveryDocDate":"2024-02-13T15:05:49","deliveryDocNum":"MyDocumentNumber","trackingNumber":"MyTrackingNumber","basePriceDelivery":1999.99,"comments":"My new comment for manager","responsibleId":1,"xmlId":"myNewXmlId"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipment.update
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.shipment.update', {
            id: 2452,
            fields: {
                allowDelivery: 'N',
                deducted: 'N',
                deliveryId: 3,
                statusId: 'DD',
                deliveryDocDate: '2024-02-13T15:05:49',
                deliveryDocNum: 'MyDocumentNumber',
                trackingNumber: 'MyTrackingNumber',
                basePriceDelivery: 1999.99,
                comments: 'My new comment for manager',
                responsibleId: 1,
                xmlId: 'myNewXmlId',
            }
        },
        function(result) {
            if (result.error())
                console.error(result.error().ex);
            else
                console.log(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.shipment.update',
        [
            'id' => 2452,
            'fields' => [
                'allowDelivery' => 'N',
                'deducted' => 'N',
                'deliveryId' => 3,
                'statusId' => 'DD',
                'deliveryDocDate' => '2024-02-13T15:05:49',
                'deliveryDocNum' => 'MyDocumentNumber',
                'trackingNumber' => 'MyTrackingNumber',
                'basePriceDelivery' => 1999.99,
                'comments' => 'My new comment for manager',
                'responsibleId' => 1,
                'xmlId' => 'myNewXmlId',
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
         "allowDelivery":"N",
         "basePriceDelivery":1999.99,
         "canceled":"N",
         "comments":"My new comment for manager",
         "companyId":null,
         "currency":"USD",
         "customPriceDelivery":"N",
         "dateAllowDelivery":"2024-04-12T10:01:23+03:00",
         "dateCanceled":null,
         "dateDeducted":"2024-04-12T10:01:23+03:00",
         "dateInsert":"2024-04-11T14:17:52+03:00",
         "dateMarked":null,
         "dateResponsibleId":"2024-04-12T10:01:23+03:00",
         "deducted":"N",
         "deliveryDocDate":"2024-02-13T14:05:49+03:00",
         "deliveryDocNum":"MyDocumentNumber",
         "deliveryId":3,
         "deliveryName":"Pickup",
         "deliveryXmlId":"",
         "discountPrice":0,
         "empAllowDeliveryId":1,
         "empCanceledId":null,
         "empDeductedId":1,
         "empMarkedId":null,
         "empResponsibleId":1,
         "externalDelivery":"N",
         "id":2452,
         "id1c":"",
         "marked":"N",
         "orderId":2068,
         "priceDelivery":1999.99,
         "reasonMarked":"",
         "reasonUndoDeducted":"",
         "responsibleId":1,
         "shipmentItems":[
            
         ],
         "statusId":"DD",
         "statusXmlId":"",
         "system":"N",
         "trackingDescription":"",
         "trackingLastCheck":"",
         "trackingNumber":"MyTrackingNumber",
         "trackingStatus":"",
         "updated1c":"N",
         "version1c":"",
         "xmlId":"myNewXmlId"
      }
   },
   "time":{
      "start":1712928678.417617,
      "finish":1712928679.68092,
      "duration":1.2633028030395508,
      "processing":1.0808379650115967,
      "date_start":"2024-04-12T16:31:18+03:00",
      "date_finish":"2024-04-12T16:31:19+03:00"
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
[`sale_order_shipment`](../data-types.md) | Object containing information about the updated shipment ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":0,
   "error_description":"Required fields: name"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201140400001` | Shipment not found ||
|| `200040300020` | Insufficient permissions to update the shipment ||
|| `BX_INVALID_VALUE` | Value of one of the fields did not pass validation before saving ||
|| `100` | Parameter `id` not specified ||
|| `100` | Parameter `fields` not specified or empty ||
|| `0` | Required fields in the `fields` structure not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-shipment-add.md)
- [{#T}](./sale-shipment-get.md)
- [{#T}](./sale-shipment-list.md)
- [{#T}](./sale-shipment-delete.md)
- [{#T}](./sale-shipment-get-fields.md)