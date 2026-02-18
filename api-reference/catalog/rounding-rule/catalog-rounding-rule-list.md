# Get a list of price rounding rules by filter catalog.roundingRule.list

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method returns a list of price rounding rules.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select** 
[`array`](../../data-types.md)| An array with a list of fields to select (see fields of the object [catalog_rounding_rule](../data-types.md#catalog_rounding_rule)).

If the array is not provided or an empty array is passed, all available fields of the price rounding rule will be selected.
||
|| **filter** 
[`object`](../../data-types.md)| An object for filtering the selected price rounding rules in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_rounding_rule](../data-types.md#catalog_rounding_rule).

An additional prefix can be specified for the key to clarify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search goes from both sides.
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
[`object`](../../data-types.md)| An object for sorting the selected price rounding rules in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the object [catalog_rounding_rule](../data-types.md#catalog_rounding_rule).

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order
||
|| **start** 
[`integer`](../../data-types.md)| This parameter is used to manage pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the value of the `start` parameter:

`start = (N-1) * 50`, where `N` is the desired page number.
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
    -d '{"select":["id","price","roundType"],"filter":{"modifiedBy":1},"order":{"id":"ASC"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.roundingrule.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","price","roundType"],"filter":{"modifiedBy":1},"order":{"id":"ASC"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.roundingrule.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'catalog.roundingrule.list',
        {
          select: ['id', 'price', 'roundType'],
          filter: { 'modifiedBy': 1 },
          order: { 'id': 'ASC' }
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
      const generator = $b24.fetchListMethod('catalog.roundingrule.list', {
        select: ['id', 'price', 'roundType'],
        filter: { 'modifiedBy': 1 },
        order: { 'id': 'ASC' }
      }, 'ID');
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity); }
      }
    } catch (error) {
      console.error('Request failed', error);
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('catalog.roundingrule.list', {
        select: ['id', 'price', 'roundType'],
        filter: { 'modifiedBy': 1 },
        order: { 'id': 'ASC' }
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
                'catalog.roundingrule.list',
                [
                    'select' => [
                        'id',
                        'price',
                        'roundType'
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
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error listing rounding rules: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.roundingrule.list',
        {
            select:[
                'id',
                'price',
                'roundType'
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
        'catalog.roundingrule.list',
        [
            'select' => [
                'id',
                'price',
                'roundType'
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
    "roundingRules": [
        {
            "id": 1,
            "price": 1500,
            "roundType": 2
        },
        {
            "id": 2,
            "price": 1000,
            "roundType": 4
        },
        {
            "id": 4,
            "price": 50,
            "roundType": 1
        }
    ],
    "total": 3,
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
|| **roundingRules**
[`catalog_rounding_rule[]`](../data-types.md#catalog_rounding_rule) | An array of objects with information about the selected price rounding rules ||
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
|| `200040300010` | Insufficient rights to read
||
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-rounding-rule-add.md)
- [{#T}](./catalog-rounding-rule-update.md)
- [{#T}](./catalog-rounding-rule-get.md)
- [{#T}](./catalog-rounding-rule-delete.md)
- [{#T}](./catalog-rounding-rule-get-fields.md)

