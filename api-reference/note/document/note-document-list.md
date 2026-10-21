# Get a List of Documents note.document.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`note`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `note.document.list` method returns, page by page, all documents available to the user across all knowledge bases.

{% note info "" %}

The method returns document metadata without the text. To retrieve the document text, use the [note.document.get](./note-document-get.md) method.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter**
[`array`](../../data-types.md) | List filter in REST 3.0 format: an array of conditions of the form `["field", "operator", "value"]`. [Filter field description](#filter).

The filter syntax is described in the [REST 3.0 overview](../../rest-v3.md#filter).

By default, only active documents from all available knowledge bases are returned ||
|| **pagination**
[`object`](../../data-types.md) | Pagination object. [Object structure description](#pagination).

Do not pass the parameter for the first page ||
|| **select**
[`array`](../../data-types.md) | List of document fields to return in the response. Field names match the response fields, for example `["id","title","state"]`.

The `markdown` field cannot be passed: the method returns a validation error.

By default, all fields from the [Returned Data](#returned-data) section are returned ||
|#

### Parameter Filter {#filter}

#|
|| **Field** | **Description** ||
|| **collectionId**
[`integer`](../../data-types.md) | Knowledge base identifier. The value `0` returns documents without a knowledge base — documents in the Recycle Bin whose knowledge base has been permanently deleted.

The identifier can be obtained using the [note.collection.list](../collection/note-collection-list.md) method ||
|| **state**
[`string`](../../data-types.md) | Document state. The `=` and `in` operators are supported.

Possible values:

- `live` — active document
- `archived` — archived document
- `trashed` — document in the Recycle Bin

Default: `live`.

A `state` condition can be specified only at the top level of the filter. Inside `or` logic or a negation, the condition causes a validation error.

Documents in the Recycle Bin from a knowledge base that is itself in the Recycle Bin are returned only to a user with the "Administration" permission for that knowledge base ||
|#

### Parameter Pagination {#pagination}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../../data-types.md) | Number of documents per page.

Allowed values: from `1` to `200`. Values above `200` are reduced to `200`; zero and negative values are replaced with `50`. A nonnumeric value causes a validation error.

Default: `50` ||
|| **afterCursor**
[`object`](../../data-types.md) | Next page cursor. Pass the `result.nextCursor` value from the previous response unchanged.

The cursor structure is internal and may change. Do not build the cursor manually.

The order of documents in the response is not guaranteed, but a full traversal using cursors returns each document exactly once ||
|#

{% note warning "" %}

Stop the traversal only when `result.nextCursor` is `null`. An empty `result.items` array with a non-null cursor does not mean the end of the list: if you stop the loop on an empty page, some documents will be skipped.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.document.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["collectionId","=",47]],"pagination":{"limit":2}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.document.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["collectionId","=",47]],"pagination":{"limit":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.document.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentListResult = {
      items: Array<{
        id: number
        collectionId: number | null
        parentId: number | null
        title: string
        position: number
        createdBy: number
        updatedBy: number
        createdAt: ISODate
        updatedAt: ISODate
        contentUpdatedAt: ISODate | null
        state: 'live' | 'archived' | 'trashed'
        isArchived: boolean
        isTrashed: boolean
        isOrphan: boolean
      }>
      nextCursor: Record<string, unknown> | null
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentListResult>({
        method: 'note.document.list',
        params: {
          filter: [
            ['collectionId', '=', 47],
          ],
          pagination: {
            limit: 2,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Documents:', result.items.length, result.nextCursor)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listDocuments() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'note.document.list',
            params: {
              filter: [
                ['collectionId', '=', 47],
              ],
              pagination: {
                limit: 2,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Documents:', result.items.length, result.nextCursor)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listDocuments)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.note.document.list(
            filter=[
                ["collectionId", "=", 47],
            ],
            pagination={
                "limit": 2,
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'note.document.list',
                [
                    'filter' => [
                        ['collectionId', '=', 47],
                    ],
                    'pagination' => [
                        'limit' => 2,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'note.document.list',
        {
            filter: [
                ['collectionId', '=', 47],
            ],
            pagination: {
                limit: 2,
            },
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'note.document.list',
        [
            'filter' => [
                ['collectionId', '=', 47],
            ],
            'pagination' => [
                'limit' => 2,
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "note.document.list", b24.Params{
    	"filter": []any{
    		[]any{"collectionId", "=", 47},
    	},
    	"pagination": b24.Params{
    		"limit": 2,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("note.document.list: %w", err)
    }

    var result struct {
    	Items []struct {
    		ID    int    `json:"id"`
    		Title string `json:"title"`
    		State string `json:"state"`
    	} `json:"items"`
    	// Pass the cursor to the next request unchanged.
    	NextCursor json.RawMessage `json:"nextCursor"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range result.Items {
    	fmt.Println(item.ID, item.Title, item.State)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "nextCursor": {
            "stream": "collections",
            "collectionId": 47,
            "id": 127
        },
        "items": [
            {
                "id": 129,
                "collectionId": 47,
                "parentId": 125,
                "title": "Vacation request",
                "position": 1000,
                "createdBy": 1,
                "updatedBy": 1,
                "createdAt": "2026-10-21T15:30:10+03:00",
                "updatedAt": "2026-10-21T15:30:10+03:00",
                "contentUpdatedAt": "2026-10-21T15:30:10+03:00",
                "state": "live",
                "isArchived": false,
                "isTrashed": false,
                "isOrphan": false
            },
            {
                "id": 127,
                "collectionId": 47,
                "parentId": null,
                "title": "VPN setup guide",
                "position": 2000,
                "createdBy": 1,
                "updatedBy": 1,
                "createdAt": "2026-10-21T15:30:05+03:00",
                "updatedAt": "2026-10-21T15:30:05+03:00",
                "contentUpdatedAt": "2026-10-21T15:30:05+03:00",
                "state": "live",
                "isArchived": false,
                "isTrashed": false,
                "isOrphan": false
            }
        ]
    },
    "time": {
        "start": 1792585819,
        "finish": 1792585819.986398,
        "duration": 0.9863979816436768,
        "processing": 0,
        "date_start": "2026-10-21T15:30:19+03:00",
        "date_finish": "2026-10-21T15:30:19+03:00",
        "operating_reset_at": 1792586400,
        "operating": 0
    }
}
```

### Returned Data {#returned-data}

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object containing a list of documents and a cursor ||
|| **result.items**
[`array`](../../data-types.md) | Array of documents on the page. It may be empty even if `nextCursor` is not `null` ||
|| **result.items[].id**
[`integer`](../../data-types.md) | Document identifier ||
|| **result.items[].collectionId**
[`integer`](../../data-types.md) or `null` | Knowledge base identifier. `null` if the user has access only to the document itself, not to the knowledge base ||
|| **result.items[].parentId**
[`integer`](../../data-types.md) or `null` | Parent document identifier. `null` for a top-level document ||
|| **result.items[].title**
[`string`](../../data-types.md) | Document title ||
|| **result.items[].position**
[`integer`](../../data-types.md) | Sort weight among sibling documents ||
|| **result.items[].createdBy**
[`integer`](../../data-types.md) | Document author identifier ||
|| **result.items[].updatedBy**
[`integer`](../../data-types.md) | Identifier of the user who last modified the document record ||
|| **result.items[].createdAt**
[`datetime`](../../data-types.md) | Creation date and time in ISO 8601 format with a timezone offset ||
|| **result.items[].updatedAt**
[`datetime`](../../data-types.md) | Date and time the document record was modified: renaming, moving, archiving, or overwriting via REST ||
|| **result.items[].contentUpdatedAt**
[`datetime`](../../data-types.md) or `null` | Date and time the Markdown representation of the text was generated. Changes after edits in the editor ||
|| **result.items[].state**
[`string`](../../data-types.md) | Document state: `live` — active, `archived` — archived, `trashed` — in the Recycle Bin ||
|| **result.items[].isArchived**
[`boolean`](../../data-types.md) | Deprecated field. Use `state`. `true` — the document is in the archive ||
|| **result.items[].isTrashed**
[`boolean`](../../data-types.md) | Deprecated field. Use `state`. `true` — the document is in the Recycle Bin ||
|| **result.items[].isOrphan**
[`boolean`](../../data-types.md) | Deprecated field. `true` — the document is in the Recycle Bin and its knowledge base has been deleted ||
|| **result.nextCursor**
[`object`](../../data-types.md) or `null` | Next page cursor. Pass it to `pagination.afterCursor` unchanged. `null` means there are no more pages ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "markdown is not a field of this list: it answers with the metainformation of a document and never with its text. Read the text of one document with note.document.get.",
                "field": "select"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | markdown is not a field of this list: it answers with the metainformation of a document and never with its text. Read the text of one document with note.document.get. | Remove `markdown` from `select`. To retrieve the document text, use the [note.document.get](./note-document-get.md) method ||
|| `state` | unknown state: expected one of live, archived, trashed | Pass `live`, `archived`, or `trashed` ||
|| `state` | operator "#OPERATOR#" is not supported for state: use "=" or "in" | Use the `=` or `in` operator for `state` ||
|| `state` | state belongs to the top-level "and" of the filter: it cannot sit under "or" or under a negation | Move the `state` condition to the top level of the filter ||
|| `pagination.limit` | must be a number | Pass a number ||
|| `afterCursor.id` | must be a number | Pass the cursor from `result.nextCursor` unchanged ||
|| `afterCursor.collectionId` | must be a number | Pass the cursor from `result.nextCursor` unchanged ||
|| `stream` | unknown stream: expected one of collections, grants, orphans | Pass the cursor from `result.nextCursor` unchanged ||
|#

#### Filter Format Error

Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter` | Invalid filter "Unknown filter condition" | Pass `filter` as an array of conditions, for example `[["collectionId","=",47]]` ||
|#

#### Unknown Field Error

Error code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unknown field "#FIELD#" found in entity "DocumentItemDto" | In `select`, specify only fields from the [Returned Data](#returned-data) section ||
|#

#### Access Errors

Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | The user does not have access to the Knowledge Base module ||
|#

#### Object Not Found Error

Error code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `collectionId` | Entity with ID #ID# was not found | The knowledge base from the filter does not exist or is not available to the user. Check the identifier using the [note.collection.list](../collection/note-collection-list.md) method ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-document-get.md)
- [{#T}](./note-document-tree-list.md)
- [{#T}](./note-document-search-list.md)
- [{#T}](./index.md)
