# Get a List of Warehouse Accounting Documents catalog.document.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" access permission

The method `catalog.document.list` returns a paginated list of warehouse accounting documents. By default, filters are applied to the request, limiting the selection to the available document types and the current user's permissions.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array of fields from [catalog_document](../data-types.md#catalog_document) that need to be selected.

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
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol must be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol must be included in the value. Examples:
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

To select the second page of results, pass the value `50`. To select the third page of results, pass the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` is the desired page number. 
Or pass the value from the `next` key in the response. ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DocumentListResult = {
      documents: {
        docType: string
        id: number
        status: string
        title: string
      }[]
    }

    try {
      // catalog.document.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<DocumentListResult>({
        method: 'catalog.document.list',
        params: {
          select: ['id', 'docType', 'title', 'status'],
          filter: {
            '>=dateCreate': '2025-10-01T00:00:00+02:00',
            '<=dateCreate': '2025-10-15T23:59:59+02:00',
          },
          order: { id: 'ASC' },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Documents on this page:', result.documents.length, result.documents)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function fetchDocumentList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // catalog.document.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'catalog.document.list',
            params: {
              select: ['id', 'docType', 'title', 'status'],
              filter: {
                '>=dateCreate': '2025-10-01T00:00:00+02:00',
                '<=dateCreate': '2025-10-15T23:59:59+02:00',
              },
              order: { id: 'ASC' },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Documents on this page:', result.documents.length, result.documents)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchDocumentList)
    </script>
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
            order:  { id: 'ASC' }
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
                result.next(); // substitutes the value from the response into start and repeats the request
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
                "title": "Receipt #2"
            },
            {
                "docType": "A",
                "id": 7,
                "status": "N",
                "title": "Test Rest"
            },
            // ...other documents
            {
                "docType": "S",
                "id": 105,
                "status": "N",
                "title": "receipt 10"
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
[`object`](../../data-types.md) | The root element of the response ||
|| **documents**
[`catalog_document[]`](../data-types.md#catalog_document) | A list of documents, the response structure depends on the `select` parameter ||
|| **next**
[`integer`](../../data-types.md) | Offset pointer for the next page. Pass this value to the `start` parameter to get the next 50 records ||
|| **total**
[`integer`](../../data-types.md) | The total number of documents ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

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

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-document-update.md)
- [{#T}](./document-element/catalog-document-element-list.md)
- [{#T}](../documentcontractor/catalog-documentcontractor-list.md)
