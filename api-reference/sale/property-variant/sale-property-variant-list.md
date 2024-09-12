# Get a List of Property Variants sale.propertyvariant.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method retrieves a list of property variant values. It is applicable only for properties of type `ENUM`.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of fields to select (see fields of the [sale_order_property_variant](../data-types.md) object).

If the array is not provided or an empty array is passed, all available fields of the property variant values will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected property variant values in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [sale_order_property_variant](../data-types.md) object.

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `+` — filter by the exact value of the specified field; this will also include elements where the field value is undefined (NULL)
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The % symbol in the filter value does not need to be passed. The search looks for a substring at any position in the string
- `!=` — not equal
- `!` — not equal
||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected property variant values in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [sale_order_property_variant](../data-types.md) object.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
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
    -d '{"select":["id","name","orderPropsId","value"],"filter":{">=id":5},"order":{"orderPropsId":"desc","id":"asc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertyvariant.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","orderPropsId","value"],"filter":{">=id":5},"order":{"orderPropsId":"desc","id":"asc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertyvariant.list
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.propertyvariant.list", {
            "select": ["id", "name", "orderPropsId", "value"],
            "filter": {
                ">=id": 5,
            },
            "order": {
                "orderPropsId": "desc",
                "id": "asc",
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
        'sale.propertyvariant.list',
        [
            'select' => ['id', 'name', 'orderPropsId', 'value'],
            'filter' => ['>=id' => 5],
            'order' => [
                'orderPropsId' => 'desc',
                'id' => 'asc',
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
        "propertyVariants":[
            {
                "id":8,
                "name":"M",
                "orderPropsId":50,
                "value":"m"
            },
            {
                "id":9,
                "name":"L",
                "orderPropsId":50,
                "value":"l"
            },
            {
                "id":6,
                "name":"Red",
                "orderPropsId":49,
                "value":"red"
            },
            {
                "id":7,
                "name":"Green",
                "orderPropsId":49,
                "value":"green"
            }
        ]
    },
    "total":4,
    "time":{
        "start":1711633054.631642,
        "finish":1711633054.872368,
        "duration":0.24072599411010742,
        "processing":0.011013984680175781,
        "date_start":"2024-03-28T16:37:34+03:00",
        "date_finish":"2024-03-28T16:37:34+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **propertyVariants**
[`sale_order_property_variant[]`](../data-types.md) | An array of objects containing information about the selected property variant values ||
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
|| `200040300010` | Insufficient permissions to read property variant values ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-variant-add.md)
- [{#T}](./sale-property-variant-update.md)
- [{#T}](./sale-property-variant-get.md)
- [{#T}](./sale-property-variant-delete.md)
- [{#T}](./sale-property-variant-get-fields.md)