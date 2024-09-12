# Add Property Variant sale.propertyvariant.add

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a variant value for a property. It is applicable only for properties of type `ENUM`.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating a property variant value ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **orderPropsId***
[`sale_order_property.id`](../data-types.md) | Property identifier ||
|| **name***
[`string`](../../data-types.md) | Name of the property variant value ||
|| **value***
[`string`](../../data-types.md) | Symbolic code of the property variant value ||
|| **sort**
[`integer`](../../data-types.md) | Sorting ||
|| **description**
[`string`](../../data-types.md) | Description of the property variant value ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Red","orderPropsId":49,"value":"red","sort":10,"description":"Description of the value for red color"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertyvariant.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Red","orderPropsId":49,"value":"red","sort":10,"description":"Description of the value for red color"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertyvariant.add
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.propertyvariant.add", {
            "fields": {
                "name": "Red",
                "orderPropsId": 49,
                "value": "red",
                "sort": 10,
                "description": "Description of the value for red color"
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
        'sale.propertyvariant.add',
        [
            'fields' => [
                'name' => 'Red',
                'orderPropsId' => 49,
                'value' => 'red',
                'sort' => 10,
                'description' => 'Description of the value for red color'
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
            "description":"Description of the value for red color",
            "id":5,
            "name":"Red",
            "orderPropsId":49,
            "sort":10,
            "value":"red"
        }
    },
    "time":{
        "start":1711629310.006284,
        "finish":1711629310.334167,
        "duration":0.3278830051422119,
        "processing":0.024754047393798828,
        "date_start":"2024-03-28T15:35:10+03:00",
        "date_finish":"2024-03-28T15:35:10+03:00"
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
[`sale_order_property_variant`](../data-types.md) | Object containing information about the added property variant value ||
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
|| `201550000003` | Property not found for which the property variant value is being added ||
|| `200040300020` | Insufficient permissions to add the property variant value ||
|| `100` | Parameter `fields` is not specified or is empty ||
|| `0` | Required fields of the `fields` structure are not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|| `ERROR_NO_VALUE` | Empty value for the symbolic code of the property variant value provided ||
|| `ERROR_NO_NAME` | Empty value for the name of the property variant value provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-variant-update.md)
- [{#T}](./sale-property-variant-get.md)
- [{#T}](./sale-property-variant-list.md)
- [{#T}](./sale-property-variant-delete.md)
- [{#T}](./sale-property-variant-get-fields.md)