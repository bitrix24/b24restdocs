# Get List of Shipments sale.shipment.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of shipments.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see fields of the [sale_order_shipment](../data-types.md) object).
 ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected payment bindings to shipments in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.
 
Possible values for `field` correspond to the fields of the [sale_order_shipment](../data-types.md) object.

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:

- `=` — equals (works with arrays as well)
- `%` — LIKE, substring search. The % symbol in the filter value does not need to be sent. The search looks for the substring in any position of the string.
- `>` — greater than
- `<` — less than
- `!=` — not equal
- `!%` — NOT LIKE, substring search. The % symbol in the filter value does not need to be sent. The search goes from both sides.
- `>=` — greater than or equal to
- `<=` — less than or equal to
- `=%` — LIKE, substring search. The % symbol needs to be sent in the value. Examples: 
    - `"mol%"` — searching for values starting with "mol"
    - `"%mol"` — searching for values ending with "mol"
    - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!=%` — NOT LIKE, substring search. The % symbol needs to be sent in the value. Examples:
    - `"mol%"` — searching for values not starting with "mol"
    - `"%mol"` — searching for values not ending with "mol"
    - `"%mol%"` — searching for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (see description above)
||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected payment bindings to shipments in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.
 
Possible values for `field` correspond to the fields of the [sale_order_shipment](../data-types.md) object.
 
Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
 ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used for managing pagination.
 
The page size of results is always static: 50 records.
 
To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.
 
The formula for calculating the value of the `start` parameter:
 
`start = (N-1) * 50`, where `N` — the number of the desired page
 ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","accountNumber","allowDelivery","basePriceDelivery","canceled","comments","companyId","currency","customPriceDelivery","dateAllowDelivery","dateCanceled","dateDeducted","dateInsert","dateMarked","dateResponsibleId","deducted","deliveryDocDate","deliveryDocNum","deliveryId","deliveryName","deliveryXmlId","discountPrice","empAllowDeliveryId","empCanceledId","empDeductedId","empMarkedId","empResponsibleId","externalDelivery","id1c","marked","orderId","priceDelivery","reasonMarked","reasonUndoDeducted","responsibleId","statusId","statusXmlId","system","trackingDescription","trackingLastCheck","trackingNumber","trackingStatus","updated1c","version1c","xmlId"],"filter":{"@orderId":[2069,2070],">=id":2464},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipment.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","accountNumber","allowDelivery","basePriceDelivery","canceled","comments","companyId","currency","customPriceDelivery","dateAllowDelivery","dateCanceled","dateDeducted","dateInsert","dateMarked","dateResponsibleId","deducted","deliveryDocDate","deliveryDocNum","deliveryId","deliveryName","deliveryXmlId","discountPrice","empAllowDeliveryId","empCanceledId","empDeductedId","empMarkedId","empResponsibleId","externalDelivery","id1c","marked","orderId","priceDelivery","reasonMarked","reasonUndoDeducted","responsibleId","statusId","statusXmlId","system","trackingDescription","trackingLastCheck","trackingNumber","trackingStatus","updated1c","version1c","xmlId"],"filter":{"@orderId":[2069,2070],">=id":2464},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipment.list
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.shipment.list", {
            "select": [
                "id",
                "accountNumber",
                "allowDelivery",
                "basePriceDelivery",
                "canceled",
                "comments",
                "companyId",
                "currency",
                "customPriceDelivery",
                "dateAllowDelivery",
                "dateCanceled",
                "dateDeducted",
                "dateInsert",
                "dateMarked",
                "dateResponsibleId",
                "deducted",
                "deliveryDocDate",
                "deliveryDocNum",
                "deliveryId",
                "deliveryName",
                "deliveryXmlId",
                "discountPrice",
                "empAllowDeliveryId",
                "empCanceledId",
                "empDeductedId",
                "empMarkedId",
                "empResponsibleId",
                "externalDelivery",
                "id1c",
                "marked",
                "orderId",
                "priceDelivery",
                "reasonMarked",
                "reasonUndoDeducted",
                "responsibleId",
                "statusId",
                "statusXmlId",
                "system",
                "trackingDescription",
                "trackingLastCheck",
                "trackingNumber",
                "trackingStatus",
                "updated1c",
                "version1c",
                "xmlId",
            ],
            "filter": {
                ">=id": 2464,
                "@orderId": [2069, 2070],
            },
            "order": {
                "id": "desc",
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
        'sale.shipment.list',
        [
            'select' => [
                "id",
                "accountNumber",
                "allowDelivery",
                "basePriceDelivery",
                "canceled",
                "comments",
                "companyId",
                "currency",
                "customPriceDelivery",
                "dateAllowDelivery",
                "dateCanceled",
                "dateDeducted",
                "dateInsert",
                "dateMarked",
                "dateResponsibleId",
                "deducted",
                "deliveryDocDate",
                "deliveryDocNum",
                "deliveryId",
                "deliveryName",
                "deliveryXmlId",
                "discountPrice",
                "empAllowDeliveryId",
                "empCanceledId",
                "empDeductedId",
                "empMarkedId",
                "empResponsibleId",
                "externalDelivery",
                "id1c",
                "marked",
                "orderId",
                "priceDelivery",
                "reasonMarked",
                "reasonUndoDeducted",
                "responsibleId",
                "statusId",
                "statusXmlId",
                "system",
                "trackingDescription",
                "trackingLastCheck",
                "trackingNumber",
                "trackingStatus",
                "updated1c",
                "version1c",
                "xmlId",
            ],
            'filter' => [
                ">=id" => 2464,
                "@orderId" => [2069, 2070],
            ],
            'order' => [
                "id" => "desc",
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
      "shipments":[
         {
            "accountNumber":"2070\/2",
            "allowDelivery":"N",
            "basePriceDelivery":0,
            "canceled":"N",
            "comments":"",
            "companyId":0,
            "currency":"USD",
            "customPriceDelivery":"N",
            "dateAllowDelivery":null,
            "dateCanceled":null,
            "dateDeducted":null,
            "dateInsert":"2024-04-11T16:59:20+03:00",
            "dateMarked":null,
            "dateResponsibleId":"2024-04-11T16:59:20+03:00",
            "deducted":"N",
            "deliveryDocDate":null,
            "deliveryDocNum":"",
            "deliveryId":1,
            "deliveryName":"No Delivery",
            "deliveryXmlId":"",
            "discountPrice":null,
            "empAllowDeliveryId":null,
            "empCanceledId":null,
            "empDeductedId":null,
            "empMarkedId":null,
            "empResponsibleId":null,
            "externalDelivery":"N",
            "id":2467,
            "id1c":"",
            "marked":"N",
            "orderId":2070,
            "priceDelivery":0,
            "reasonMarked":"",
            "reasonUndoDeducted":"",
            "responsibleId":0,
            "statusId":"DN",
            "statusXmlId":"FFdddd",
            "system":"N",
            "trackingDescription":"",
            "trackingLastCheck":"",
            "trackingNumber":"",
            "trackingStatus":"",
            "updated1c":"N",
            "version1c":"",
            "xmlId":"bx_6617fac818ee2"
         },
         {
            "accountNumber":"2069\/2",
            "allowDelivery":"N",
            "basePriceDelivery":600,
            "canceled":"N",
            "comments":"",
            "companyId":0,
            "currency":"USD",
            "customPriceDelivery":"N",
            "dateAllowDelivery":null,
            "dateCanceled":null,
            "dateDeducted":null,
            "dateInsert":"2024-04-11T15:05:48+03:00",
            "dateMarked":null,
            "dateResponsibleId":"2024-04-11T15:05:48+03:00",
            "deducted":"N",
            "deliveryDocDate":null,
            "deliveryDocNum":"",
            "deliveryId":2,
            "deliveryName":"Courier Delivery",
            "deliveryXmlId":"",
            "discountPrice":null,
            "empAllowDeliveryId":null,
            "empCanceledId":null,
            "empDeductedId":null,
            "empMarkedId":null,
            "empResponsibleId":null,
            "externalDelivery":"N",
            "id":2465,
            "id1c":"",
            "marked":"N",
            "orderId":2069,
            "priceDelivery":600,
            "reasonMarked":"",
            "reasonUndoDeducted":"",
            "responsibleId":0,
            "statusId":"DN",
            "statusXmlId":"FFdddd",
            "system":"N",
            "trackingDescription":"",
            "trackingLastCheck":"",
            "trackingNumber":"",
            "trackingStatus":"",
            "updated1c":"N",
            "version1c":"",
            "xmlId":"bx_6617e02cae2a1"
         }
      ]
   },
   "total":2,
   "time":{
      "start":1712847615.641893,
      "finish":1712847615.857499,
      "duration":0.2156059741973877,
      "processing":0.02844095230102539,
      "date_start":"2024-04-11T18:00:15+03:00",
      "date_finish":"2024-04-11T18:00:15+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **shipments**
[`sale_order_shipment[]`](../data-types.md) | An array of objects with information about the selected shipments ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
   "error":0,
   "error_description":"error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read shipments ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-shipment-add.md)
- [{#T}](./sale-shipment-update.md)
- [{#T}](./sale-shipment-get.md)
- [{#T}](./sale-shipment-delete.md)
- [{#T}](./sale-shipment-get-fields.md)