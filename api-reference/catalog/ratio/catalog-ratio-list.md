# Get the list of measurement unit ratios catalog.ratio.list

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns a list of measurement unit ratios.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | 
An array with the list of fields to select (see the fields of the [catalog_ratio](../data-types.md#catalog_ratio) object) 
||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected measurement unit ratios in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_ratio](../data-types.md#catalog_ratio) object. 

An additional prefix can be set for the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
||
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected fields of measurement unit ratios in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_ratio](../data-types.md#catalog_ratio) object.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to control pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","productId","ratio","isDefault"],"filter":{"@productId":[1,2],">ratio":0.5,"isDefault":"Y"},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.ratio.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","productId","ratio","isDefault"],"filter":{"@productId":[1,2],">ratio":0.5,"isDefault":"Y"},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.ratio.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'catalog.ratio.list',
        {
          select: ['id', 'productId', 'ratio', 'isDefault'],
          filter: {
            '@productId': [1, 2],
            '>ratio': 0.5,
            'isDefault': 'Y',
          },
          order: {
            id: 'desc',
          },
        },
        (progress) => { console.log('Progress:', progress) }
      );
      const items = response.getData() || [];
      for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('catalog.ratio.list', {
        select: ['id', 'productId', 'ratio', 'isDefault'],
        filter: {
          '@productId': [1, 2],
          '>ratio': 0.5,
          'isDefault': 'Y',
        },
        order: {
          id: 'desc',
        },
      }, 'ID');
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity); }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('catalog.ratio.list', {
        select: ['id', 'productId', 'ratio', 'isDefault'],
        filter: {
          '@productId': [1, 2],
          '>ratio': 0.5,
          'isDefault': 'Y',
        },
        order: {
          id: 'desc',
        },
      }, 0);
      const result = response.getData().result || [];
      for (const entity of result) { console.log('Entity:', entity); }
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
                'catalog.ratio.list',
                [
                    'select' => [
                        'id',
                        'productId',
                        'ratio',
                        'isDefault',
                    ],
                    'filter' => [
                        '@productId' => [1, 2],
                        '>ratio'     => 0.5,
                        'isDefault'  => 'Y',
                    ],
                    'order' => [
                        'id' => 'desc',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching ratio list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.ratio.list',
            {
                select:[
                    'id',
                    'productId',
                    'ratio',
                    'isDefault',
                ],
                filter:{
                    '@productId': [1, 2],
                    '>ratio': 0.5,
                    'isDefault': 'Y',
                },
                order:{
                    id: 'desc',
                },
            },
            function(result)
            {
                if(result.error()) {
                    console.error(result.error());
                } else {
                    console.log(result.data());
                }
            }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.ratio.list',
        [
            'select' => [
                'id',
                'productId',
                'ratio',
                'isDefault',
            ],
            'filter' => [
                '@productId' => [1, 2],
                '>ratio' => 0.5,
                'isDefault' => 'Y',
            ],
            'order' => [
                'id' => 'desc',
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
        "ratios": [
            {
                "id": 1,
                "isDefault": "Y",
                "productId": 1,
                "ratio": 1
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1729676806.653016,
        "finish": 1729676807.083635,
        "duration": 0.4306190013885498,
        "processing": 0.02678680419921875,
        "date_start": "2024-10-23T12:46:46+02:00",
        "date_finish": "2024-10-23T12:46:47+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **ratios**
[`catalog_ratio[]`](../data-types.md#catalog_ratio) | An array of objects with information about the selected measurement unit ratios ||
|| **total**
[`integer`](../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":200040300010,
    "error_description":"Access denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read the trade catalog
||
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-ratio-get.md)
- [{#T}](./catalog-ratio-get-fields.md)

