# Accessing Fields of the sale.shipmentitem.get Element

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The `sale.shipmentitem.get` method is designed to retrieve the values of all fields of the shipment line item.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_shipment_item.id`](../data-types.md) | Identifier of the shipment line item ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipmentitem.get
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipmentitem.get
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.shipmentitem.get", {
            "id": 7
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
        'sale.shipmentitem.get',
        [
            'id' => 7
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result":{
        "shipmentItem":{
            "basketId":2716,
            "dateInsert":"2024-04-11T09:10:34+03:00",
            "id":7,
            "orderDeliveryId":2431,
            "quantity":5,
            "reservedQuantity":0,
            "xmlId":"myNewXmlId"
        }
    },
    "time":{
        "start":1712819691.140072,
        "finish":1712819691.534972,
        "duration":0.394899845123291,
        "processing":0.21921396255493164,
        "date_start":"2024-04-11T10:14:51+03:00",
        "date_finish":"2024-04-11T10:14:51+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **shipmentItem**
[`sale_order_shipment_item`](../data-types.md) | Information about the shipment line item ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":201240400001,
    "error_description":"shipment item does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201240400001` | Shipment line item not found ||
|| `200040300010` | Insufficient permissions to read the shipment line item ||
|| `100` | Parameter `id` not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./sale-shipment-item-add.md)
- [{#T}](./sale-shipment-item-update.md)
- [{#T}](./sale-shipment-item-list.md)
- [{#T}](./sale-shipment-item-delete.md)
- [{#T}](./sale-shipment-item-get-fields.md)