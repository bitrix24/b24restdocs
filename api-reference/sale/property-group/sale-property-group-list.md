# Get a List of Property Groups sale.propertygroup.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method is designed to retrieve a list of property groups.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of fields to select (see fields of the object [sale_order_property_group](../data-types.md)).

If the array is not provided or an empty array is passed, all available fields of property groups will be selected.
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected property groups in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [sale_order_property_group](../data-types.md). 

An additional prefix can be set for the key to specify the filter behavior. Possible prefix values:
- `+` — filtering by the exact value of the specified field; this also includes elements where the field value is undefined (NULL)
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The % symbol in the filter value does not need to be passed. The search looks for a substring at any position in the string
- `!=` — not equal
- `!` — not equal
||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected property groups in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [sale_order_property_group](../data-types.md).

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
    -d '{"select":["id","name","personTypeId","sort"],"filter":{">=id":14},"order":{"name":"asc","id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertygroup.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","personTypeId","sort"],"filter":{">=id":14},"order":{"name":"asc","id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertygroup.list
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.propertygroup.list", {
            "select": ["id", "name", "personTypeId", "sort"],
            "filter": {
                ">=id": 14,
            },
            "order": {
                "name": "asc",
                "id": "desc",
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
        'sale.propertygroup.list',
        [
            'select' => ['id', 'name', 'personTypeId', 'sort'],
            'filter' => ['>=id' => 14],
            'order' => [
                'name' => 'asc',
                'id' => 'desc',
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
        "propertyGroups":[
            {
                "id":18,
                "name":"New Property Group 2",
                "personTypeId":3,
                "sort":100
            },
            {
                "id":14,
                "name":"New Property Group 1",
                "personTypeId":3,
                "sort":100
            }
        ]
    },
    "total":2,
    "time":{
        "start":1711544498.747502,
        "finish":1711544498.987554,
        "duration":0.2400519847869873,
        "processing":0.010115861892700195,
        "date_start":"2024-03-27T16:01:38+03:00",
        "date_finish":"2024-03-27T16:01:38+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **propertyGroups**
[`sale_order_property_group[]`](../data-types.md) | Array of objects with information about the selected property groups ||
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
|| `200040300010` | Insufficient permissions to read property groups ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-property-group-add.md)
- [{#T}](./sale-property-group-update.md)
- [{#T}](./sale-property-group-get.md)
- [{#T}](./sale-property-group-delete.md)
- [{#T}](./sale-property-group-get-fields.md)