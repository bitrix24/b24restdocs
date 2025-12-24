# Get a list of vendor bindings to documents catalog.documentcontractor.list

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the following permissions:
> — "View" on the document type "Incoming",
> — "View Warehouse Accounting section"
> — "View Product Catalog"    

The method `catalog.documentcontractor.list` returns a list of vendor bindings to warehouse accounting documents. By default, filters are added to the request that limit the selection based on the current user's permissions.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** || 
|| **select**
[`array`](../../data-types.md) | An array of fields from [catalog_documentcontractor](../data-types.md#catalog_documentcontractor) that need to be selected.

If the array is not provided or an empty array is passed, all available fields of the document will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected bindings in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_documentcontractor](../data-types.md#catalog_documentcontractor) object.

An additional prefix can be specified for the key to clarify the filter's behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `=` — exact match
- `!=`, `!` — not equal
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` character should not be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` character should be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `!%` — NOT LIKE, substring search. The `%` character should not be included in the filter value. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` character should be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `=` — equal, exact match, used by default
- `!=` — not equal
- `!` — not equal
|| 
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected documents in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_documentcontractor](../data-types.md#catalog_documentcontractor) object.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order 

By default, results are sorted in ascending order by `id`. ||
|| **start**
[`integer`](../../data-types.md) | This parameter is used to manage pagination.

The page size is 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number. 
Or pass the value from the `next` key in the response. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","documentId"],"filter":{"documentId":7},"order":{"id":"ASC"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.documentcontractor.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","documentId"],"filter":{"documentId":7},"order":{"id":"ASC"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.documentcontractor.list
    ```

- JS

    ```js  
    // callListMethod: Retrieves all data at once.
    // Use only for small selections (< 1000 items) due to high
    // memory load.

    try {
    const response = await $b24.callListMethod(
        'catalog.documentcontractor.list',
        {
        select: ["id", "documentId"],
        filter: { "documentId": 7 },
        order: { "id": "ASC" },
        start: 0
        },
        (progress) => { console.log('Progress:', progress) }
    );
    const items = response.getData() || [];
    for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
    console.error('Request failed', error)
    }

    // fetchListMethod: Retrieves data in parts using an iterator.
    // Use for large volumes of data for efficient memory consumption.

    try {
    const generator = $b24.fetchListMethod('catalog.documentcontractor.list', {
        select: ["id", "documentId"],
        filter: { "documentId": 7 },
        order: { "id": "ASC" },
        start: 0
    }, 'ID');
    for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
    }
    } catch (error) {
    console.error('Request failed', error)
    }

    // callMethod: Manual control of pagination through the start parameter.
    // Use for precise control over request batches.
    // Less efficient for large data than fetchListMethod.

    try {
    const response = await $b24.callMethod('catalog.documentcontractor.list', {
        select: ["id", "documentId"],
        filter: { "documentId": 7 },
        order: { "id": "ASC" },
        start: 0
    }, 0);
    const result = response.getData().result || [];
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
                'catalog.documentcontractor.list',
                [
                    'select' => ["id", "documentId"],
                    'filter' => ['documentId' => 7],
                    'order' => ['id' => 'ASC'],
                    'start' => 0
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.documentcontractor.list',
        {
            select: ["id", "documentId"],
            filter: { "documentId": 7 },
            order: { "id": "ASC" },
            start: 0
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
                
                // If there is a next page
                if (result.more())
                {
                    console.log('Next page start: ' + result.next());
                }
            }
        }
    );
    ```	

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.documentcontractor.list',
        [
            'select' => ["id", "documentId"],
            'filter' => ['documentId' => 7],
            'order' => ['id' => 'ASC'],
            'start' => 0
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "documentContractor": [
        {
            "documentId": 7,
            "id": 5
        }
        ]
    },
    "total": 1,
    "time": {
        "start": 1766146580,
        "finish": 1766146580.866608,
        "duration": 0.8666079044342041,
        "processing": 0,
        "date_start": "2025-12-19T15:16:20+01:00",
        "date_finish": "2025-12-19T15:16:20+01:00",
        "operating_reset_at": 1766147180,
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
|| **documentContractor**
[`catalog_documentContractor[]`](../data-types.md#catalog_documentContractor) | A list of contractor bindings, the response structure depends on the `select` parameter || 
|| **next**
[`integer`](../../data-types.md) | Offset pointer for the next page. Pass the value in the `start` parameter to get the next 50 records ||
|| **total**
[`integer`](../../data-types.md) | The total number of documents ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [error handling](../../../_includes/error-info.md) %}

HTTP Code: **400**

```json
{
    "error": "0",
    "error_description": "Access denied"
}
```

### Possible Error Codes  

#|  
|| **Code** | **Description** | **Value** ||
|| `0` | Access denied | Insufficient permissions to view the document or bindings ||  
|| `0` | Contractors should be provided by CRM | The CRM module is not active as a contractor provider ||  
|# 

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-documentcontractor-add.md)  
- [{#T}](./catalog-documentcontractor-delete.md)  
- [{#T}](./catalog-documentcontractor-get-fields.md)  