# Get Available Fields of Property Value sale.propertyvalue.getFields

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method returns the available fields of the order property value options.

No parameters.

## Code Examples

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertyvalue.getFields
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertyvalue.getFields
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.propertyvalue.getFields", {},
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
        'sale.propertyvalue.getFields',
        []
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
            "orderId":{
                "isImmutable":true,
                "isReadOnly":false,
                "isRequired":true,
                "type":"integer"
            },
            "orderPropsId":{
                "isImmutable":false,
                "isReadOnly":false,
                "isRequired":true,
                "type":"integer"
            },
            "orderPropsXmlId":{
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
        "start":1712057516.862786,
        "finish":1712057517.152171,
        "duration":0.2893848419189453,
        "processing":0.0048639774322509766,
        "date_start":"2024-04-02T14:31:56+03:00",
        "date_finish":"2024-04-02T14:31:57+03:00"
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
[`object`](../../data-types.md) | Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is the identifier of the object [sale_order_property_value](../data-types.md), and `value` is an object of type [rest_field_description](../data-types.md) ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
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

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-value-modify.md)
- [{#T}](./sale-property-value-get.md)
- [{#T}](./sale-property-value-list.md)
- [{#T}](./sale-property-value-delete.md)