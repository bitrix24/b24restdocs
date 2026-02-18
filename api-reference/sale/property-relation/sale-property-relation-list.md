# Get a list of property bindings sale.propertyRelation.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.propertyRelation.list` allows you to retrieve a list of property bindings.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | The array contains a list of fields to select (see fields of the object [sale_order_property_relation](../data-types.md#sale_order_property_relation)).

If not provided or an empty array is passed, all available fields of property bindings will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected property bindings in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [sale_order_property_relation](../data-types.md#sale_order_property_relation).

An additional prefix can be assigned to the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@`— NOT IN (an array is passed as the value)
- `%` — LIKE, substring search. The `%` symbol in the filter value should not be passed. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol should be passed in the value. Examples:
    - "mol%" — searching for values starting with "mol"
    - "%mol" — searching for values ending with "mol"
    - "%mol%" — searching for values where "mol" can be in any position

- `%=` — LIKE (see description above)

- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value should not be passed. The search goes from both sides.

- `!=%` — NOT LIKE, substring search. The `%` symbol should be passed in the value. Examples:
    - "mol%" — searching for values not starting with "mol"
    - "%mol" — searching for values not ending with "mol"
    - "%mol%" — searching for values where the substring "mol" is not present in any position

- `!%=` — NOT LIKE (see description above)

- `=` — equal, exact match (used by default)
- `!=` - not equal
- `!` — not equal
 ||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected property bindings in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [sale_order_property_relation](../data-types.md#sale_order_property_relation).

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order ||
|| **start**
[`integer`](../../data-types.md) | The parameter is used to control pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["entityId","entityType","propertyId"],"filter":{"entityId":6},"order":{"entityId":"asc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.propertyRelation.list
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["entityId","entityType","propertyId"],"filter":{"entityId":6},"order":{"entityId":"asc"},    "auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.propertyRelation.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'sale.propertyRelation.list',
        {
          select: [
            'entityId',
            'entityType',
            'propertyId',
          ],
          filter: {
            entityId: 6,
          },
          order: {
            entityId: 'asc',
          },
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
      const generator = $b24.fetchListMethod('sale.propertyRelation.list', {
        select: [
          'entityId',
          'entityType',
          'propertyId',
        ],
        filter: {
          entityId: 6,
        },
        order: {
          entityId: 'asc',
        },
      }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('sale.propertyRelation.list', {
        select: [
          'entityId',
          'entityType',
          'propertyId',
        ],
        filter: {
          entityId: 6,
        },
        order: {
          entityId: 'asc',
        },
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
                'sale.propertyRelation.list',
                [
                    'select' => [
                        'entityId',
                        'entityType',
                        'propertyId',
                    ],
                    'filter' => [
                        'entityId' => 6,
                    ],
                    'order' => [
                        'entityId' => 'asc',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing sale property relations: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.propertyRelation.list', 
        {
            select:[
                'entityId',
                'entityType',
                'propertyId',
            ],
            filter:{
                entityId: 6,
            },
            order:{
                entityId: 'asc',
            },
        }, 
        function(result)
        {
            if(result.error())
                console.error(result.error());
           else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.propertyRelation.list',
        [
            'select' => [
                'entityId',
                'entityType',
                'propertyId',
            ],
            'filter' => [
                'entityId' => 6,
            ],
            'order' => [
                'entityId' => 'asc',
            ],
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
        "propertyRelations": [
            {
                "entityId": 6,
                "entityType": "L",
                "propertyId": 39
            },
            {
                "entityId": 6,
                "entityType": "D",
                "propertyId": 40
            },
            {
                "entityId": 6,
                "entityType": "L",
                "propertyId": 40
            },
        ]
    },
    "total": 3,
    "time": {
        "start": 1712308456.66363,
        "finish": 1712308457.096419,
        "duration": 0.4327890872955322,
        "processing": 0.023340940475463867,
        "date_start": "2024-04-05T12:14:16+02:00",
        "date_finish": "2024-04-05T12:14:17+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **propertyRelations**
[`sale_order_property_relation`](../data-types.md) | An array of objects with information about the selected property bindings ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":200040300010,
    "error_description":"Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read available fields of property bindings ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./sale-property-relation-add.md)
- [{#T}](./sale-property-relation-delete-by-filter.md)
- [{#T}](./sale-property-relation-get-fields.md)

