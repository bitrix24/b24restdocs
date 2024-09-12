# Get Available Fields of the Property Variant sale.propertyvariant.getFields

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns the available fields of property value variants.

No parameters.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertyvariant.getFields
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertyvariant.getFields
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.propertyvariant.getFields", {},
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

    $result = CRest::call('sale.propertyvariant.getFields', []);

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
        "propertyVariant":{
            "description":{
                "isImmutable":false,
                "isReadOnly":false,
                "isRequired":false,
                "type":"string"
            },
            "id":{
                "isImmutable":false,
                "isReadOnly":true,
                "isRequired":false,
                "type":"integer"
            },
            "name":{
                "isImmutable":false,
                "isReadOnly":false,
                "isRequired":true,
                "type":"string"
            },
            "orderPropsId":{
                "isImmutable":true,
                "isReadOnly":false,
                "isRequired":true,
                "type":"integer"
            },
            "sort":{
                "isImmutable":false,
                "isReadOnly":false,
                "isRequired":false,
                "type":"integer"
            },
            "value":{
                "isImmutable":false,
                "isReadOnly":false,
                "isRequired":true,
                "type":"string"
            },
            "xnlId":{
                "isImmutable":false,
                "isReadOnly":false,
                "isRequired":false,
                "type":"string"
            }
        }
    },
    "time":{
        "start":1711634366.714184,
        "finish":1711634366.95501,
        "duration":0.24082589149475098,
        "processing":0.0055849552154541016,
        "date_start":"2024-03-28T16:59:26+03:00",
        "date_finish":"2024-03-28T16:59:26+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **propertyVariant**
[`object`](../../data-types.md) | Object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is the identifier of the object [sale_order_property_variant](../data-types.md), and `value` is an object of type [rest_field_description](../data-types.md) ||
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
|| `200040300010` | Insufficient permissions to read available fields of property value variants ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-variant-add.md)
- [{#T}](./sale-property-variant-update.md)
- [{#T}](./sale-property-variant-list.md)
- [{#T}](./sale-property-variant-get.md)
- [{#T}](./sale-property-variant-delete.md)