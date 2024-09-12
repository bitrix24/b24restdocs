# Get Available Fields of Shipment Property Values sale.shipmentpropertyvalue.getfields

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method returns the available fields of shipment property values.

No parameters.

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.shipmentpropertyvalue.getfields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.shipmentpropertyvalue.getfields
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.shipmentpropertyvalue.getfields", {},
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
        'sale.shipmentpropertyvalue.getfields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Successful Response

HTTP Status: **200**

```json
{
    "result":{
        "propertyValue":{
            "code":{
                "isImmutable":false,
                "isReadOnly":false,
                "isRequired":false,
                "type":"string"
            },
            "id":{
                "isImmutable":true,
                "isReadOnly":true,
                "isRequired":false,
                "type":"integer"
            },
            "name":{
                "isImmutable":false,
                "isReadOnly":false,
                "isRequired":false,
                "type":"string"
            },
            "shipmentId":{
                "isImmutable":true,
                "isReadOnly":false,
                "isRequired":true,
                "type":"integer"
            },
            "shipmentPropsId":{
                "isImmutable":false,
                "isReadOnly":false,
                "isRequired":true,
                "type":"integer"
            },
            "shipmentPropsXmlId":{
                "isImmutable":false,
                "isReadOnly":true,
                "isRequired":false,
                "type":"string"
            },
            "value":{
                "isImmutable":false,
                "isReadOnly":false,
                "isRequired":true,
                "type":"string"
            }
        }
    },
    "time":{
        "start":1718024003.807242,
        "finish":1718024003.98344,
        "duration":0.17619800567626953,
        "processing":0.005009889602661133,
        "date_start":"2024-06-10T15:53:23+03:00",
        "date_finish":"2024-06-10T15:53:23+03:00"
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
[`object`](../../data-types.md) | Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is the identifier of the object [sale_shipment_property_value](../data-types.md#sale_shipment_property_value), and `value` is an object of type [rest_field_description](../../data-types.md) ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

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
|| `200040300020` | Insufficient permissions to read available fields of property values ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include notitle [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-shipment-property-value-modify.md)
- [{#T}](./sale-shipment-property-value-get.md)
- [{#T}](./sale-shipment-property-value-list.md)
- [{#T}](./sale-shipment-propertyvalue-delete.md)