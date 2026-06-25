# Find Documents note.document.search.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`note`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module

The `note.document.search.list` method searches for documents by heading and content.

{% note info "" %}

The response contains documents from Knowledge bases to which the user has access, as well as documents with direct access. Archived documents are not included in the results.

{% endnote %}

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **query***
[`string`](../../../data-types.md) | Search query.

Minimum length: `3` characters

Maximum length: `200` characters ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination object. [Object structure description](#pagination) ||
|#

### Parameter Pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../../data-types.md) | Page size.

Allowed values: from `1` to `200`

Default: `50` ||
|#

{% note info "" %}

The method returns only the first page of results: the `hasMore` field in the response indicates whether there are additional matches, but there is no cursor for the next page. To retrieve more matches, refine `query` or increase `limit`.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{install_address}/rest/api/{user_id}/{webhook_token}/note.document.search.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"query":"borscht recipe","pagination":{"limit":50}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.document.search.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"query":"borscht recipe","pagination":{"limit":50},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.document.search.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentSearchListResult = {
      items: Array<{
        documentId: number
        collectionId: number | null
        title: string
        score: number
        snippet: string
        sharedAccess: boolean
      }>
      hasMore: boolean
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentSearchListResult>({
        method: 'note.document.search.list',
        params: {
          query: 'borscht recipe',
          pagination: {
            limit: 50,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Found documents:', result.items.length, result.hasMore)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function searchDocuments() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'note.document.search.list',
            params: {
              query: 'borscht recipe',
              pagination: {
                limit: 50,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Found documents:', result.items.length, result.hasMore)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', searchDocuments)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'note.document.search.list',
                [
                    'query' => 'borscht recipe',
                    'pagination' => [
                        'limit' => 50,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error searching documents: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'note.document.search.list',
        {
            query: 'borscht recipe',
            pagination: {
                limit: 50
            }
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'note.document.search.list',
        [
            'query' => 'borscht recipe',
            'pagination' => [
                'limit' => 50
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
        "items": [
            {
                "documentId": 42,
                "collectionId": 123,
                "title": "Traditional recipe",
                "score": 0.853,
                "snippet": "...classic <b>borscht</b> recipe...",
                "sharedAccess": false
            },
            {
                "documentId": 43,
                "collectionId": null,
                "title": "Publicly available recipe",
                "score": 0.791,
                "snippet": "...quick <b>recipe</b>...",
                "sharedAccess": true
            }
        ],
        "hasMore": true
    },
    "time": {
        "start": 1780640100,
        "finish": 1780640100.286773,
        "duration": 0.2867729663848877,
        "processing": 0.24144291877746582,
        "date_start": "2026-06-19T10:15:00+03:00",
        "date_finish": "2026-06-19T10:15:00+03:00",
        "operating_reset_at": 1780640700,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with search results ||
|| **items**
[`array`](../../../data-types.md) | List of found documents ||
|| **items[]**
[`object`](../../../data-types.md) | Found document object ||
|| **documentId**
[`integer`](../../../data-types.md) | Found document identifier ||
|| **collectionId**
[`integer`](../../../data-types.md) | Knowledge base identifier or `null` if the document is available via direct access to the document ||
|| **title**
[`string`](../../../data-types.md) | Document title ||
|| **score**
[`double`](../../../data-types.md) | Relative relevance of the match ||
|| **snippet**
[`string`](../../../data-types.md) | HTML fragment with highlighted matches (tags <b>…</b>) ||
|| **sharedAccess**
[`boolean`](../../../data-types.md) | Flag for direct access to the document without access to the entire knowledge base.

Possible values:

- `true` — document is available via direct access
- `false` — document is available via knowledge base access ||
|| **hasMore**
[`boolean`](../../../data-types.md) | `true` value if there are more results beyond the current page ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "NOTE_SEARCH_QUERY_TOO_SHORT",
        "message": "Search query is too short"
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `query` | Mandatory field `query` is not specified | Add `query` to the request body ||
|| `query`
`pagination` | Field `#FIELD#` requires data type `#TYPE#` for such a request | Ensure the provided value is of the correct type ||
|#

Error Code: `NOTE_SEARCH_QUERY_TOO_SHORT`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `query` | Search query is too short | Provide a string at least `3` characters long ||
|#

Error Code: `NOTE_SEARCH_QUERY_TOO_LONG`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `query` | Search query is too long | Shorten the string to `200` characters ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | User does not have access to the Knowledge Base module ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-document-tree-list.md)
- [{#T}](./note-document-get.md)
- [{#T}](./index.md)