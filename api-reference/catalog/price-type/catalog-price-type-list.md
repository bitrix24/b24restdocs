# Get a list of price types by filter catalog.priceType.list

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns a list of price types based on the filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select** 
[`array`](../../data-types.md)| An array of fields to select (see fields of the [catalog_price_type](../data-types.md#catalog_price_type) object).

If the array is not provided or an empty array is passed, all available fields of the price type will be selected.
||
|| **filter** 
[`object`](../../data-types.md)| An object for filtering the selected price types in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_price_type](../data-types.md#catalog_price_type) object.

An additional prefix can be specified for the key to clarify the filter's behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol in the filter value should not be passed. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol should be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value should not be passed. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol should be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal
||
|| **order**
[`object`](../../data-types.md)| An object for sorting the selected price types in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_price_type](../data-types.md#catalog_price_type) object.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
||
|| **start** 
[`integer`](../../data-types.md)| This parameter is used to manage pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the value of the `start` parameter:

`start = (N-1) * 50`, where `N` is the desired page number
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
    -d '{"select":["id","name","xmlId"],"filter":{"modifiedBy":1},"order":{"id":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.priceType.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","name","xmlId"],"filter":{"modifiedBy":1},"order":{"id":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.priceType.list
    ```

- JS

    ```js
    // callListMethod is recommended when you need to retrieve the entire set of list data and the volume of records is relatively small (up to about 1000 items). The method loads all data at once, which can lead to high memory load when working with large volumes.
    
    const parameters = {
        select: ['id', 'name', 'xmlId'],
        filter: { 'modifiedBy': 1 },
        order: { 'id': 'ASC' }
    };
    
    try {
        const response = await $b24.callListMethod(
            'catalog.priceType.list',
            parameters,
            (progress) => { console.log('Progress:', progress) }
        );
        const items = response.getData() || [];
        for (const entity of items) { console.log('Entity:', entity); }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // fetchListMethod is preferable when working with large datasets. The method implements iterative fetching using a generator, allowing data to be processed in parts and efficiently using memory.
    
    const parameters = {
        select: ['id', 'name', 'xmlId'],
        filter: { 'modifiedBy': 1 },
        order: { 'id': 'ASC' }
    };
    
    try {
        const generator = $b24.fetchListMethod('catalog.priceType.list', parameters, 'ID');
        for await (const page of generator) {
            for (const entity of page) { console.log('Entity:', entity); }
        }
    } catch (error) {
        console.error('Request failed', error);
    }
    
    // callMethod provides manual control over the pagination process through the start parameter. Suitable for scenarios where precise control over request batches is required. However, it may be less efficient compared to fetchListMethod when dealing with large volumes of data.
    
    const parameters = {
        select: ['id', 'name', 'xmlId'],
        filter: { 'modifiedBy': 1 },
        order: { 'id': 'ASC' }
    };
    
    try {
        const response = await $b24.callMethod('catalog.priceType.list', parameters, 0);
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
                'catalog.priceType.list',
                [
                    'select' => [
                        'id',
                        'name',
                        'xmlId'
                    ],
                    'filter' => [
                        'modifiedBy' => 1
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
        echo 'Error fetching price types: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.priceType.list',
        {
            select:[
                'id',
                'name',
                'xmlId'
            ],
            filter:{
                'modifiedBy': 1
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
        'catalog.priceType.list',
        [
            'select' => [
                'id',
                'name',
                'xmlId'
            ],
            'filter' => [
                'modifiedBy' => 1
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
    "priceTypes": [
        {
            "id": 1,
            "name": "BASE",
            "xmlId": "BASE"
        },
        {
            "id": 2,
            "name": "Base wholesale price",
            "xmlId": "basewholesale"
        }
    ],
    "total": 2,
    "time": {
        "start": 1712326352.63409,
        "finish": 1712326352.8319,
        "duration": 0.197818040847778,
        "processing": 0.00833678245544434,
        "date_start": "2024-04-05T16:24:46+02:00",
        "date_finish": "2024-04-05T16:24:46+02:00",
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
|| **priceTypes**
[`catalog_price_type[]`](../data-types.md#catalog_price_type) | Array of objects with information about the selected price types ||
|| **total**
[`integer`](../../data-types.md#time) | Total number of records found ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
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
|| **Code** | **Description** ||
|| `200040300010` | Insufficient permissions to read
||
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-add.md)
- [{#T}](./catalog-price-type-update.md)
- [{#T}](./catalog-price-type-get.md)
- [{#T}](./catalog-price-type-delete.md)
- [{#T}](./catalog-price-type-get-fields.md)