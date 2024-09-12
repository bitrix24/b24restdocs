# Get Shipment Property Value by ID sale.shipmentpropertyvalue.get

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method returns the values of the shipment property.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_shipment_property_value.id`](../data-types.md#sale_shipment_property_value) | Identifier of the shipment property value ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":38164}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipmentpropertyvalue.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":38164,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipmentpropertyvalue.get
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.shipmentpropertyvalue.get", {
            "id": 38164
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
        'sale.shipmentpropertyvalue.get',
        [
            'id' => 38164
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
        "propertyValue":{
            "code":null,
            "id":38164,
            "name":"Comments",
            "shipmentPropsId":105,
            "value":"Comments value"
        }
    },
    "time":{
        "start":1718023082.525679,
        "finish":1718023082.798483,
        "duration":0.27280378341674805,
        "processing":0.055876970291137695,
        "date_start":"2024-06-10T15:38:02+03:00",
        "date_finish":"2024-06-10T15:38:02+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **propertyValue**
[`sale_shipment_property_value.id`](../data-types.md#sale_shipment_property_value) | Information about the shipment property value ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":201040400001,
    "error_description":"Property value has not been found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201040400001` | Property value not found ||
|| `200040300010` | Insufficient permissions to read the property value ||
|| `100` | Parameter `id` not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-shipment-property-value-modify.md)
- [{#T}](./sale-shipment-property-value-list.md)
- [{#T}](./sale-shipment-propertyvalue-delete.md)
- [{#T}](./sale-shipment-property-value-get-fields.md)