# Update Item in the Shipment Table Part sale.shipmentitem.update

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.shipmentitem.update` updates an item in the shipment table part collection.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_shipment_item.id`](../data-types.md) | Identifier of the shipment table part item ||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the shipment table part item ||
|#

### Parameter fields

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **quantity***
[`double`](../../data-types.md) | Quantity of the product ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier.

Can be used to synchronize the current delivery item with a similar item in an external system ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7,"fields":{"quantity":5,"xmlId":"myNewXmlId"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipmentitem.update
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7,"fields":{"quantity":5,"xmlId":"myNewXmlId"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipmentitem.update
    ```

- JS

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

- PHP

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
        "start":1712819636.302217,
        "finish":1712819637.183715,
        "duration":0.8814980983734131,
        "processing":0.6984810829162598,
        "date_start":"2024-04-11T10:13:56+03:00",
        "date_finish":"2024-04-11T10:13:57+03:00"
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
[`sale_order_shipment_item`](../data-types.md) | Object containing information about the updated shipment table part item ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

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
|| `201240400001` | The updated shipment table part item was not found ||
|| `200040300020` | Insufficient permissions to update the shipment table part item ||
|| `100` | The `id` parameter is missing ||
|| `100` | The `fields` parameter is missing or empty ||
|| `0` | Required fields in the `fields` structure are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./sale-shipment-item-add.md)
- [{#T}](./sale-shipment-item-get.md)
- [{#T}](./sale-shipment-item-list.md)
- [{#T}](./sale-shipment-item-delete.md)
- [{#T}](./sale-shipment-item-get-fields.md)