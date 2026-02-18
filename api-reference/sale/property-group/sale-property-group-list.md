# Get a list of property groups sale.propertygroup.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method is designed to retrieve a list of property groups.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of fields to select (see fields of the [sale_order_property_group](../data-types.md) object).

If the array is not provided or is empty, all available fields of property groups will be selected.
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected property groups in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [sale_order_property_group](../data-types.md) object.

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
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

Possible values for `field` correspond to the fields of the [sale_order_property_group](../data-types.md) object.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

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
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'sale.propertygroup.list',
        {
          "select": ["id", "name", "personTypeId", "sort"],
          "filter": {
            ">=id": 14,
          },
          "order": {
            "name": "asc",
            "id": "desc",
          }
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('sale.propertygroup.list', {
        "select": ["id", "name", "personTypeId", "sort"],
        "filter": {
          ">=id": 14,
        },
        "order": {
          "name": "asc",
          "id": "desc",
        }
      }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('sale.propertygroup.list', {
        "select": ["id", "name", "personTypeId", "sort"],
        "filter": {
          ">=id": 14,
        },
        "order": {
          "name": "asc",
          "id": "desc",
        }
      }, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.propertygroup.list',
                [
                    'select' => ['id', 'name', 'personTypeId', 'sort'],
                    'filter' => [
                        '>=id' => 14,
                    ],
                    'order' => [
                        'name' => 'asc',
                        'id'   => 'desc',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching property groups: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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

HTTP status: **200**

```json
{
    "result": {
        "propertyGroups": [
            {
                "id": 18,
                "name": "New property group 2",
                "personTypeId": 3,
                "sort": 100
            },
            {
                "id": 14,
                "name": "New property group 1",
                "personTypeId": 3,
                "sort": 100
            }
        ]
    },
    "total": 2,
    "time": {
        "start": 1711544498.747502,
        "finish": 1711544498.987554,
        "duration": 0.2400519847869873,
        "processing": 0.010115861892700195,
        "date_start": "2024-03-27T16:01:38+02:00",
        "date_finish": "2024-03-27T16:01:38+02:00"
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
[`sale_order_property_group[]`](../data-types.md) | Array of objects containing information about the selected property groups ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description": "error"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient rights to read property groups ||
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

