# Get a List of Price Type Bindings to Customer Groups catalog.priceTypeGroup.list

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View Product Catalog" or "Manage Price Types" access permission.

The method `catalog.priceTypeGroup.list` returns a list of price type bindings to customer groups.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md)| An array of fields from [catalog_price_type_group](../../data-types.md#catalog_price_type_group) that need to be selected.

If the array is not provided or an empty array is passed, all available binding fields will be selected.
||
|| **filter**
[`object`](../../data-types.md)| An object for filtering the selected bindings in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_price_type_group](../../data-types.md#catalog_price_type_group) object.

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value
- `=%` — LIKE, substring search. The `%` symbol should be included in the value
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value
- `!=%` — NOT LIKE, substring search. The `%` symbol should be included in the value
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

If the filter conditions do not match any records, the method will return an empty list.
||
|| **order**
[`object`](../../data-types.md)| An object for sorting the selected bindings in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_price_type_group](../../data-types.md#catalog_price_type_group) object.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../data-types.md)| This parameter is used for managing pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number
||
|#

## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","catalogGroupId","groupId","access"],"filter":{"catalogGroupId":9,"groupId":23},"order":{"id":"ASC"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.priceTypeGroup.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","catalogGroupId","groupId","access"],"filter":{"catalogGroupId":9,"groupId":23},"order":{"id":"ASC"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.priceTypeGroup.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory load.
    
    const parameters = {
        select: ['id', 'catalogGroupId', 'groupId', 'access'],
        filter: { 'catalogGroupId': 9, 'groupId': 23 },
        order: { 'id': 'ASC' }
    };
    
    try {
        const response = await $b24.callListMethod(
            'catalog.priceTypeGroup.list',
            parameters,
            (progress) => { console.log('Progress:', progress) }
        );
        const items = response.getData() || [];
        for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use for large volumes of data for efficient memory consumption.
    
    const parameters = {
        select: ['id', 'catalogGroupId', 'groupId', 'access'],
        filter: { 'catalogGroupId': 9, 'groupId': 23 },
        order: { 'id': 'ASC' }
    };
    
    try {
        const generator = $b24.fetchListMethod('catalog.priceTypeGroup.list', parameters, 'ID');
        for await (const page of generator) {
            for (const entity of page) { console.log('Entity:', entity); }
        }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // callMethod: Manual control of pagination through the start parameter. Use for precise control over request batches. Less efficient for large data than fetchListMethod.
    
    const parameters = {
        select: ['id', 'catalogGroupId', 'groupId', 'access'],
        filter: { 'catalogGroupId': 9, 'groupId': 23 },
        order: { 'id': 'ASC' }
    };
    
    try {
        const response = await $b24.callMethod('catalog.priceTypeGroup.list', parameters, 0);
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
                'catalog.priceTypeGroup.list',
                [
                    'select' => ['id', 'catalogGroupId', 'groupId', 'access'],
                    'filter' => [
                        'catalogGroupId' => 9,
                        'groupId' => 23
                    ],
                    'order' => [
                        'id' => 'ASC'
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        $result->next();
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing price type groups: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.priceTypeGroup.list',
        {
            select: ['id', 'catalogGroupId', 'groupId', 'access'],
            filter: {
                catalogGroupId: 9,
                groupId: 23
            },
            order: {
                id: 'ASC'
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
        'catalog.priceTypeGroup.list',
        [
            'select' => ['id', 'catalogGroupId', 'groupId', 'access'],
            'filter' => [
                'catalogGroupId' => 9,
                'groupId' => 23
            ],
            'order' => [
                'id' => 'ASC'
            ],
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
        "priceTypeGroups": [
            {
                "access": "Y",
                "catalogGroupId": 9,
                "groupId": 23,
                "id": 109
            },
            {
                "access": "N",
                "catalogGroupId": 9,
                "groupId": 23,
                "id": 111
            }
        ]
    },
    "total": 2,
    "time": {
        "start": 1774259997,
        "finish": 1774259997.152525,
        "duration": 0.1525249481201172,
        "processing": 0,
        "date_start": "2026-03-23T12:59:57+01:00",
        "date_finish": "2026-03-23T12:59:57+01:00",
        "operating_reset_at": 1774260597,
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
|| **priceTypeGroups**
[`catalog_price_type_group[]`](../../data-types.md#catalog_price_type_group) | An array of objects containing information about the selected price type bindings to customer groups, structure depends on the `select` parameter ||
|| **total**
[`integer`](../../data-types.md) | The total number of records found ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 200040300010,
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `200040300010` | Access Denied | Insufficient permissions to view the catalog ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-group-add.md)
- [{#T}](./catalog-price-type-group-delete.md)
- [{#T}](./catalog-price-type-group-get-fields.md)