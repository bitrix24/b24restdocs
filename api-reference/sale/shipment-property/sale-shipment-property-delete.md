# Delete Shipment Property sale.shipmentproperty.delete

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a shipment property.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_shipment_property.id`](../data-types.md) | Identifier of the shipment property ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":57}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipmentproperty.delete
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":57,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipmentproperty.delete
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.shipmentproperty.delete", {
            "id": 57
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
        'sale.shipmentproperty.delete',
        [
            'id' => 57
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
   "result":true,
   "time":{
      "start":1712238625.263063,
      "finish":1712238625.700109,
      "duration":0.4370460510253906,
      "processing":0.029300928115844727,
      "date_start":"2024-04-04T16:50:25+03:00",
      "date_finish":"2024-04-04T16:50:25+03:00"
   }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the property deletion ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{	
   "error":200840400001,
   "error_description":"property does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200840400001` | The shipment property to be deleted was not found ||
|| `200850000004` | Internal error while deleting the shipment property ||
|| `ERROR_CORE` | Internal error while deleting the shipment property ||
|| `200040300020` | Insufficient permissions to delete the shipment property ||
|| `100` | The `id` parameter is not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-shipment-property-add.md)
- [{#T}](./sale-shipment-property-get.md)
- [{#T}](./sale-shipment-property-list.md)
- [{#T}](./sale-shipment-property-update.md)
- [{#T}](./sale-shipment-property-get-fields-by-type.md)