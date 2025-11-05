# Get the list of inventory management documents catalog.document.list

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: user with the "View product catalog" access permission

The method `catalog.document.list` returns a paginated list of inventory management documents. By default, filters are added to the request, limiting the selection to the available document types and the current user's permissions.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array with a list of fields [catalog_document](../data-types.md#catalog_document) to be selected.

If the array is not provided or an empty array is passed, all available document fields will be selected. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering the selected documents in the format `{"field_1": "value_1", ..., "field_N": "value_N"}`.

Possible values for `field` correspond to the fields of the [catalog_document](../data-types.md#catalog_document) object.

An additional prefix can be specified for the key to clarify the filter's behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `=` — exact match
- `!=`, `!` — not equal
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol does not need to be passed in the filter value. The search looks for the substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `!%` — NOT LIKE, substring search. The `%` symbol does not need to be passed in the filter value. The search goes from both sides
- `!=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `=` — equal, exact match, used by default
- `!=` — not equal
- `!` — not equal
|| 
|| **order**
[`object`](../../data-types.md) | An object for sorting the selected documents in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to the fields of the [catalog_document](../data-types.md#catalog_document) object.

Possible values for `order`:

- `asc` — in ascending order
- `desc` — in descending order 

By default, documents are sorted in ascending order by `id`. ||
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
    -d '{
          "select": ["id","docType","docNumber","title","status","dateDocument","total"],
          "filter": {">=dateCreate":"2025-10-01T00:00:00+02:00","<=dateCreate":"2025-10-15T23:59:59+02:00"},
          "order":  {"id":"ASC"},
          "start": 50
        }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.document.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
          "select": ["id","docType","docNumber","title","status","dateDocument","total"],
          "filter": {">=dateCreate":"2025-10-01T00:00:00+02:00","<=dateCreate":"2025-10-15T23:59:59+02:00"},
          "order":  {"id":"ASC"},
          "start": 50,
          "auth":  "**put_access_token_here**"
        }' \
    https://**put_your_bitrix24_address**/rest/catalog.document.list
    ```

- JS

    ```javascript
    // callListMethod: Retrieves all data at once.
    // Use only for small selections (< 1000 items) due to high
    // memory load.

    try {
    const response = await $b24.callListMethod(
        'catalog.document.list',
        {
        select: ['id', 'docType', 'title', 'status'],
        filter: { '>=dateCreate': '2025-10-01T00:00:00+02:00', '<=dateCreate': '2025-10-15T23:59:59+02:00' },
        order: { id: 'ASC' }
        },
        (progress: number) => { console.log('Progress:', progress) }
    );
    const items = response.getData() || [];
    for (const entity of items) { console.log('Entity:', entity) }
    } catch (error: any) {
    console.error('Request failed', error)
    }

    // fetchListMethod: Selects data in parts using an iterator.
    // Use for large volumes of data for efficient memory consumption.

    try {
    const generator = $b24.fetchListMethod('catalog.document.list', {
        select: ['id', 'docType', 'title', 'status'],
        filter: { '>=dateCreate': '2025-10-01T00:00:00+02:00', '<=dateCreate': '2025-10-15T23:59:59+02:00' },
        order: { id: 'ASC' },
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
    const response = await $b24.callMethod('catalog.document.list', {
        select: ['id', 'docType', 'title', 'status'],
        filter: { '>=dateCreate': '2025-10-01T00:00:00+02:00', '<=dateCreate': '2025-10-15T23:59:59+02:00' },
        order: { id: 'ASC' },
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
                'catalog.document.list',
                [
                    'select' => ['id', 'docType', 'docNumber', 'title', 'status', 'dateDocument', 'total'],
                    'filter' => [
                        '>=dateCreate' => '2025-10-01T00:00:00+02:00',
                        '<=dateCreate' => '2025-10-15T23:59:59+02:00',
                    ],
                    'order'  => ['ID' => 'ASC'],
                    'start'  => 50, // value of next from the previous response
                ]
            );

        $payload = $response
            ->getResponseData()
            ->getResult();

        print_r($payload['documents']);

        $next = $response
            ->getResponseData()
            ->getNext();

        echo PHP_EOL . 'next: ' . ($next ?? 'null');
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error loading documents: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.document.list',
        {
            select: ['id', 'docType', 'title', 'status'],
            filter: { '>=dateCreate': '2025-10-01T00:00:00+02:00', '<=dateCreate': '2025-10-15T23:59:59+02:00' },
            order:  { id: 'ASC' },
            start:  '50'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
                return;
            }

            console.table(result.data().documents);

            if (result.more())
            {
                result.next(); // will substitute the value from the response into start and repeat the request
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.document.list',
        [
            'select' => ['id', 'docType', 'docNumber', 'title', 'status', 'dateDocument', 'total'],
            'filter' => [
                '>=dateCreate' => '2025-10-01T00:00:00+02:00',
                '<=dateCreate' => '2025-10-15T23:59:59+02:00',
            ],
            'order'  => ['ID' => 'ASC'],
            'start'  => 50,
        ]
    );

    echo '<PRE>';
    print_r($result['result']['documents']);
    echo PHP_EOL . 'next: ' . ($result['next'] ?? 'null');
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "documents": [
            {
                "docType": "S",
                "id": 1,
                "status": "Y",
                "title": "Stock adjustment #2"
            },
            {
                "docType": "A",
                "id": 7,
                "status": "N",
                "title": "Test rest"
            },
            // ...other documents
            {
                "docType": "S",
                "id": 105,
                "status": "N",
                "title": "stock adjustment 10"
            }
        ]
    },
    "next": 50,
    "total": 143,
    "time": {
        "start": 1761914886,
        "finish": 1761914886.802491,
        "duration": 0.8024909496307373,
        "processing": 0,
        "date_start": "2025-10-31T15:48:06+02:00",
        "date_finish": "2025-10-31T15:48:06+02:00",
        "operating_reset_at": 1761915486,
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
|| **documents**
[`array`](../data-types.md#catalog_document) | List of documents, the response structure depends on the `select` parameter ||
|| **next**
[`integer`](../../data-types.md) | Offset pointer for the next page. Pass the value to the `start` parameter to get the next 50 records ||
|| **total**
[`integer`](../../data-types.md) | Total number of documents ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [error handling](../../../_includes/error-info.md) %}

HTTP Code: **400**

```json
{
    "error": "0",
    "error_description": "Insufficient permissions to save the document"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Insufficient permissions to save the document | The user does not have permission to view ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-update.md)
- [{#T}](./document-element/catalog-document-element-list.md)
- [{#T}](../documentcontractor/catalog-documentcontractor-list.md)