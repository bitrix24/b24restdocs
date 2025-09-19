# Get a list of inventory balances by filter catalog.storeproduct.list

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: user with "View product catalog" access permission

The method `catalog.storeproduct.list` returns a list of product inventory balances by filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select** 
[`array`](../../data-types.md)| An array with a list of fields from [catalog_storeproduct](../data-types.md#catalog_storeproduct) that need to be selected.

If the array is not provided or an empty array is passed, all available fields of the inventory will be selected.
||
|| **filter** 
[`object`](../../data-types.md)| An object for filtering the selected product inventory balances in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_storeproduct](../data-types.md#catalog_storeproduct) object.

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `=` — equal, exact match, default filter
- `!=` — not equal
- `!` — not equal
||
|| **order**
[`object`](../../data-types.md)| An object for sorting the selected inventory balances in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_storeproduct](../data-types.md#catalog_storeproduct) object.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
||
|| **start** 
[`integer`](../../data-types.md)| This parameter is used to manage pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number
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
    -d '{"select":["id","productId","storeId","amount"],"filter":{"productId":6973},"order":{"id":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.storeproduct.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","productId","storeId","amount"],"filter":{"productId":6973},"order":{"id":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.storeproduct.list
    ```

- JS
  
    ```javascript
    // callListMethod: Retrieves all data at once.
    // Use only for small selections (< 1000 items) due to high
    // memory load.

    try {
    const response = await $b24.callListMethod(
        'catalog.storeproduct.list',
        {
        select: [
            'id',
            'productId',
            'storeId',
            'amount'
        ],
        filter: {
            'productId': 6973
        },
        order: {
            'id': 'ASC'
        }
        },
        (progress: number) => { console.log('Progress:', progress) }
    );
    const items = response.getData() || [];
    for (const entity of items) { console.log('Entity:', entity) }
    } catch (error: any) {
    console.error('Request failed', error)
    }

    // fetchListMethod: Retrieves data in parts using an iterator.
    // Use for large volumes of data for efficient memory consumption.

    try {
    const generator = $b24.fetchListMethod('catalog.storeproduct.list', {
        select: [
        'id',
        'productId',
        'storeId',
        'amount'
        ],
        filter: {
        'productId': 6973
        },
        order: {
        'id': 'ASC'
        }
    }, 'ID');
    for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
    }
    } catch (error: any) {
    console.error('Request failed', error)
    }

    // callMethod: Manual control of pagination through the start parameter.
    // Use for precise control over request batches.
    // Less efficient for large data than fetchListMethod.

    try {
    const response = await $b24.callMethod('catalog.storeproduct.list', {
        select: [
        'id',
        'productId',
        'storeId',
        'amount'
        ],
        filter: {
        'productId': 6973
        },
        order: {
        'id': 'ASC'
        }
    }, 0);
    const result = response.getData().result || [];
    for (const entity of result) { console.log('Entity:', entity) }
    } catch (error: any) {
    console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.storeproduct.list',
                [
                    'select' => [
                        'id',
                        'productId',
                        'storeId',
                        'amount'
                    ],
                    'filter' => [
                        'productId' => 6973
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
        echo 'Error fetching store products: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.storeproduct.list',
        {
            select:[
                'id',
                'productId',
                'storeId',
                'amount'
            ],
            filter:{
                'productId': 6973
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
        'catalog.storeproduct.list',
        [
            'select' => [
                'id',
                'productId',
                'storeId',
                'amount'
            ],
            'filter' => [
                'productId' => 6973
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

HTTP status: **200**

```json
{
    "result": {
        "storeProducts": [
            {
                "amount": 54,
                "id": 11,
                "productId": 6973,
                "storeId": 1
            },
            {
                "amount": 14,
                "id": 13,
                "productId": 6973,
                "storeId": 11
            }
        ]
    },
    "total": 2,
    "time": {
        "start": 1758089258.945514,
        "finish": 1758089258.976911,
        "duration": 0.031397104263305664,
        "processing": 0.003245115280151367,
        "date_start": "2025-09-17T09:07:38+02:00",
        "date_finish": "2025-09-17T09:07:38+02:00",
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
|| **storeProducts**
[`catalog_storeproduct[]`](../data-types.md#catalog_storeproduct) | An array of objects containing information about the selected product inventory balances, structure depends on the `select` parameter ||
|| **total**
[`integer`](../../data-types.md#time) | Total number of records found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

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
|| `200040300010` | Access Denied | Insufficient rights to read ||
|| `0` | | Other errors || 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-store-product-get.md)
- [{#T}](./catalog-store-product-get-fields.md)