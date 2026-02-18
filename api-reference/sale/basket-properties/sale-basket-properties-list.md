# Get the list of basket properties sale.basketproperties.list

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: store manager

The method returns a set of properties for the items (positions) in the basket, selected by filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see fields of the object [`sale_basket_item_property`](../data-types.md#sale_basket_item_property)).

If the array is not provided or an empty array is passed, all available fields of the item properties (positions) in the basket will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected catalog sections in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [`sale_basket_item_property`](../data-types.md#sale_basket_item_property).

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as the value)
- `!@` — NOT IN (an array is passed as the value)
- `%` — LIKE, substring search. The % symbol in the filter value does not need to be passed. The search looks for a substring in any position of the string.
- `=%` — LIKE, substring search. The % symbol needs to be passed in the value. Examples:
  - `"mol%"` — searching for values starting with "mol"
  - `"%mol"` — searching for values ending with "mol"
  - `"%mol%"` — searching for values where "mol" can be in any position
- `%=` — LIKE (see description above)
- `!%` — NOT LIKE, substring search. The % symbol in the filter value does not need to be passed. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The % symbol needs to be passed in the value. Examples:
  - `"mol%"` — searching for values not starting with "mol"
  - `"%mol"` — searching for values not ending with "mol"
  - `"%mol%"` — searching for values where the substring "mol" is not in any position
- `!%=` – NOT LIKE (see description above)
- `=` exact match (used by default)
- `!=` — not equal
- `!` — not equal
||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected groups of properties in the format `{"field_1": "order_1", ... "field_N": "order_N"}`. 
Possible values for `field` correspond to the fields of the object [`sale_basket_item_property`](../data-types.md#sale_basket_item_property).

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.
The page size of results is always static - 50 records.
To select the second page of results, you need to pass the value - 50. To select the third page of results, the value - 100, and so on.
The formula for calculating the start parameter value:
start = (N-1) * 50, where N is the desired page number
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
    -d '{"select":["id","basketId","value","name","code"],"filter":{"basketId":6806},"order":{"id":"desc"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.basketproperties.list
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","basketId","value","name","code"],"filter":{"basketId":6806},"order":{"id":"desc"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.basketproperties.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'sale.basketproperties.list',
        {
          select: [
            'id',
            'basketId',
            'value',
            'name',
            'code',
          ],
          filter: {
            'basketId': 6806,
          },
          order: {
            id: 'desc',
          },
          start: 0,
        }
      );
      const items = response.getData() || [];
      for (const entity of items) {
        console.log('Entity:', entity);
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('sale.basketproperties.list', {
        select: [
          'id',
          'basketId',
          'value',
          'name',
          'code',
        ],
        filter: {
          'basketId': 6806,
        },
        order: {
          id: 'desc',
        },
        start: 0,
      }, 'ID');
      for await (const page of generator) {
        for (const entity of page) {
          console.log('Entity:', entity);
        }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('sale.basketproperties.list', {
        select: [
          'id',
          'basketId',
          'value',
          'name',
          'code',
        ],
        filter: {
          'basketId': 6806,
        },
        order: {
          id: 'desc',
        },
        start: 0,
      }, 0);
      const result = response.getData().result || [];
      for (const entity of result) {
        console.log('Entity:', entity);
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.basketproperties.list',
                [
                    'select' => [
                        'id',
                        'basketId',
                        'value',
                        'name',
                        'code',
                    ],
                    'filter' => [
                        'basketId' => 6806,
                    ],
                    'order' => [
                        'id' => 'desc',
                    ],
                    'start' => 0,
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
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "sale.basketproperties.list",
        {
            select: [
                'id',
                'basketId',
                'value',
                'name',
                'code',
            ],
            filter: {
                'basketId': 6806,
            },
            order: {
                id: 'desc',
            },
            start: 0,
        },
    )
        .then(
            function(result)
            {
                if (result.error())
                {
                    console.error(result.error());
                }
                else
                {
                    console.log(result.data);
                }
            },
            function(error)
            {
                console.info(error);
            }
        );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.basketproperties.list',
        [
            'select' => ['id', 'basketId', 'value', 'name', 'code'],
            'filter' => ['basketId' => 6806],
            'order' => ['id' => 'desc'],
            'start' => 0
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
    "result": {
        "basketProperties": [
            {
                "basketId": 6806,
                "code": "PARTNUM",
                "id": 18,
                "name": "Batch Number",
                "value": "F45TK266"
            },
            {
                "basketId": 6806,
                "code": "ARTICUL",
                "id": 17,
                "name": "Article",
                "value": "123-456-789"
            }
        ]
    },
    "total": 2,
    "time": {
        "start": 1714052872.323321,
        "finish": 1714052872.818371,
        "duration": 0.49504995346069336,
        "processing": 0.0272219181060791,
        "date_start": "2024-04-25T15:47:52+02:00",
        "date_finish": "2024-04-25T15:47:52+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | The root element of the response ||
|| **basketProperties**
[`sale_basket_item_property[]`](../data-types.md#sale_basket_item_property) | An array of objects with information about the selected properties of items (positions) in the basket in orders ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
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
|| `200040300010` | Insufficient permissions to read  ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./sale-basket-properties-add.md)
- [{#T}](./sale-basket-properties-update.md)
- [{#T}](./sale-basket-properties-get.md)
- [{#T}](./sale-basket-properties-delete.md)
- [{#T}](./sale-basket-properties-get-fields.md)

