# Get a List of Prices by Filter catalog.price.list

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: user with the "View product catalog" access permission

The method `catalog.price.list` returns a list of product prices based on the filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select** 
[`array`](../../data-types.md)| An array of fields from [catalog_price](../data-types.md#catalog_price) that need to be selected.

If the array is not provided or an empty array is passed, all available price fields will be selected.
||
|| **filter** 
[`object`](../../data-types.md)| An object for filtering the selected product prices in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_price](../data-types.md#catalog_price) object.

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
||
|| **order**
[`object`](../../data-types.md)| An object for sorting the selected product prices in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_price](../data-types.md#catalog_price) object.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
||
|| **start** 
[`integer`](../../data-types.md)| This parameter is used to manage pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the value of the `start` parameter:

`start = (N-1) * 50`, where `N` — the number of the desired page
||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","productId","catalogGroupId","price","currency"],"filter":{"productId":1},"order":{"id":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.price.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","productId","catalogGroupId","price","currency"],"filter":{"productId":1},"order":{"id":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.price.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    const parameters = {
        select: ['id', 'productId', 'catalogGroupId', 'price', 'currency'],
        filter: { 'productId': 1 },
        order: { 'id': 'ASC' }
    };
    
    try {
        const response = await $b24.callListMethod(
            'catalog.price.list',
            parameters,
            (progress) => { console.log('Progress:', progress) }
        );
        const items = response.getData() || [];
        for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    const parameters = {
        select: ['id', 'productId', 'catalogGroupId', 'price', 'currency'],
        filter: { 'productId': 1 },
        order: { 'id': 'ASC' }
    };
    
    try {
        const generator = $b24.fetchListMethod('catalog.price.list', parameters, 'ID');
        for await (const page of generator) {
            for (const entity of page) { console.log('Entity:', entity); }
        }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    const parameters = {
        select: ['id', 'productId', 'catalogGroupId', 'price', 'currency'],
        filter: { 'productId': 1 },
        order: { 'id': 'ASC' }
    };
    
    try {
        const response = await $b24.callMethod('catalog.price.list', parameters, 0);
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
                'catalog.price.list',
                [
                    'select' => [
                        'id',
                        'productId',
                        'catalogGroupId',
                        'price',
                        'currency'
                    ],
                    'filter' => [
                        'productId' => 1
                    ],
                    'order' => [
                        'id' => 'ASC'
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        $result->next();

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching prices: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.price.list',
        {
            select:[
                'id',
                'productId',
                'catalogGroupId',
                'price',
                'currency'
            ],
            filter:{
                'productId': 1
            },
            order:{
                'id': 'ASC'
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
            result.next();
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.price.list',
        [
            'select' => [
                'id',
                'productId',
                'catalogGroupId',
                'price',
                'currency'
            ],
            'filter' => [
                'productId' => 1
            ],
            'order' => [
                'id' => 'ASC'
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
    "result": {
        "prices": [
            {
                "catalogGroupId": 3,
                "currency": "USD",
                "id": 381,
                "price": 50,
                "productId": 6461
            },
            {
                "catalogGroupId": 1,
                "currency": "EUR",
                "id": 787,
                "price": 100.5,
                "productId": 6461
            }
        ]
    },
    "total": 2,
    "time": {
        "start": 1756796975.535775,
        "finish": 1756796975.569925,
        "duration": 0.034150123596191406,
        "processing": 0.0042951107025146484,
        "date_start": "2025-09-02T10:09:35+02:00",
        "date_finish": "2025-09-02T10:09:35+02:00",
        "operating_reset_at": 1756797575,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **prices**
[`catalog_price[]`](../data-types.md#catalog_price) | An array of objects with information about the selected product prices, structure depends on the `select` parameter ||
|| **total**
[`integer`](../../data-types.md#time) | Total number of records found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 200040300010,
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `200040300010` | Access Denied | Insufficient permissions to read ||
|| `0` | | Other errors || 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-add.md)
- [{#T}](./catalog-price-update.md)
- [{#T}](./catalog-price-get.md)
- [{#T}](./catalog-price-delete.md)
- [{#T}](./catalog-price-get-fields.md)
- [{#T}](./catalog-price-modify.md)

