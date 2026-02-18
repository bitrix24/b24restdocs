# Get a list of property variants sale.propertyvariant.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method retrieves a list of property value variants. This method is applicable only for properties of type `ENUM`.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of fields to be selected (see fields of the object [sale_order_property_variant](../data-types.md)).

If the array is not provided or an empty array is passed, all available fields of the property value variants will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected property value variants in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [sale_order_property_variant](../data-types.md).

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
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
[`object`](../../data-types.md) | An object for sorting the selected property value variants in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [sale_order_property_variant](../data-types.md).

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
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'sale.propertyvariant.list',
        {
          "select": ["id", "name", "orderPropsId", "value"],
          "filter": {
            ">=id": 5,
          },
          "order": {
            "orderPropsId": "desc",
            "id": "asc",
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
      const generator = $b24.fetchListMethod('sale.propertyvariant.list', {
        "select": ["id", "name", "orderPropsId", "value"],
        "filter": {
          ">=id": 5,
        },
        "order": {
          "orderPropsId": "desc",
          "id": "asc",
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
      const response = await $b24.callMethod('sale.propertyvariant.list', {
        "select": ["id", "name", "orderPropsId", "value"],
        "filter": {
          ">=id": 5,
        },
        "order": {
          "orderPropsId": "desc",
          "id": "asc",
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
                'sale.propertyvariant.list',
                [
                    'select' => ['id', 'name', 'orderPropsId', 'value'],
                    'filter' => [
                        '>=id' => 5,
                    ],
                    'order' => [
                        'orderPropsId' => 'desc',
                        'id' => 'asc',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching property variants: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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

HTTP status: **200**

```json
{
    "result": {
        "propertyVariants": [
            {
                "id": 8,
                "name": "M",
                "orderPropsId": 50,
                "value": "m"
            },
            {
                "id": 9,
                "name": "L",
                "orderPropsId": 50,
                "value": "l"
            },
            {
                "id": 6,
                "name": "Red",
                "orderPropsId": 49,
                "value": "red"
            },
            {
                "id": 7,
                "name": "Green",
                "orderPropsId": 49,
                "value": "green"
            }
        ]
    },
    "total": 4,
    "time": {
        "start": 1711633054.631642,
        "finish": 1711633054.872368,
        "duration": 0.24072599411010742,
        "processing": 0.011013984680175781,
        "date_start": "2024-03-28T16:37:34+02:00",
        "date_finish": "2024-03-28T16:37:34+02:00"
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
[`sale_order_property_variant[]`](../data-types.md) | Array of objects containing information about the selected property value variants ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
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
|| `200040300010` | Insufficient permissions to read property value variants ||
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

