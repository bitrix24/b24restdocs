# Update Property Variant Fields sale.propertyvariant.update

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the value variant of a property. It is applicable only for properties of type `ENUM`.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`sale_order_property_variant.id`](../data-types.md) | Identifier of the property value variant ||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the property value variant ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Name of the property value variant ||
|| **value***
[`string`](../../data-types.md) | Value (code) of the property value variant ||
|| **sort**
[`integer`](../../data-types.md) | Sorting ||
|| **description**
[`string`](../../data-types.md) | Description of the property value variant ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":5,"fields":{"name":"Red","value":"red","sort":10,"description":"New description for the red color value"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertyvariant.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":5,"fields":{"name":"Red","value":"red","sort":10,"description":"New description for the red color value"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertyvariant.update
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.propertyvariant.update", {
            "id": 5,
            "fields": {
                "name": "Red",
                "value": "red",
                "sort": 10,
                "description": "New description for the red color value"
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
        'sale.propertyvariant.update',
        [
            'id' => 5,
            'fields' => [
                'name' => 'Red',
                'value' => 'red',
                'sort' => 10,
                'description' => 'New description for the red color value'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result":{
        "propertyVariant":{
            "description":"New description for the red color value",
            "id":5,
            "name":"Red",
            "orderPropsId":49,
            "sort":10,
            "value":"red"
        }
    },
    "time":{
        "start":1711630589.257634,
        "finish":1711630589.527446,
        "duration":0.26981210708618164,
        "processing":0.010741949081420898,
        "date_start":"2024-03-28T15:56:29+03:00",
        "date_finish":"2024-03-28T15:56:29+03:00"
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
[`sale_order_property_variant`](../data-types.md) | Object with information about the updated property value variant ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
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
|| `201540400001` | The property value variant being updated was not found ||
|| `200040300020` | Insufficient permissions to update the property value variant ||
|| `100` | The `id` parameter is not specified ||
|| `100` | The `fields` parameter is not specified or is empty ||
|| `0` | Required fields in the `fields` structure were not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|| `ERROR_NO_VALUE` | An empty value was provided for the character code of the property value variant ||
|| `ERROR_NO_NAME` | An empty value was provided for the name of the property value variant ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-variant-add.md)
- [{#T}](./sale-property-variant-get.md)
- [{#T}](./sale-property-variant-list.md)
- [{#T}](./sale-property-variant-delete.md)
- [{#T}](./sale-property-variant-get-fields.md)