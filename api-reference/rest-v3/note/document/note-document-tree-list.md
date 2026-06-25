# Get Document Tree note.document.tree.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`note`](../../../scopes/permissions.md)
>
> Who can execute the method: a User with access to the Knowledge base module and "View" permissions for the document knowledge base

The `note.document.tree.list` method returns the document tree of a single knowledge base.

{% note info "" %}

Archived documents are not included in the response.

{% endnote %}

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **collectionId***
[`integer`](../../../data-types.md) | Knowledge base identifier.

The identifier can be obtained using the [note.collection.list](../collection/note-collection-list.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.document.tree.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"collectionId":123}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.document.tree.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"collectionId":123,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.document.tree.list
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type TreeNode = {
      id: number
      collectionId: number
      parentId: number | null
      title: string
      position: number
      children: TreeNode[]
    }

    type DocumentTreeListResult = {
      items: TreeNode[]
      truncated: boolean
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentTreeListResult>({
        method: 'note.document.tree.list',
        params: {
          collectionId: 123,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Tree roots:', result.items.length, result.truncated)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getDocumentTree() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'note.document.tree.list',
            params: {
              collectionId: 123,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Tree roots:', result.items.length, result.truncated)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getDocumentTree)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'note.document.tree.list',
                [
                    'collectionId' => 123,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting document tree: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'note.document.tree.list',
        {
            collectionId: 123
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
        'note.document.tree.list',
        [
            'collectionId' => 123
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
                "id": 10,
                "collectionId": 123,
                "parentId": null,
                "title": "Introduction",
                "position": 1,
                "children": [
                    {
                        "id": 11,
                        "collectionId": 123,
                        "parentId": 10,
                        "title": "Chapter 1",
                        "position": 1,
                        "children": []
                    }
                ]
            }
        ],
        "truncated": false
    },
    "time": {
        "start": 1780639500,
        "finish": 1780639500.268512,
        "duration": 0.2685120105743408,
        "processing": 0.22631406784057617,
        "date_start": "2026-06-19T10:05:00+03:00",
        "date_finish": "2026-06-19T10:05:00+03:00",
        "operating_reset_at": 1780640100,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the document tree ||
|| **items**
[`array`](../../../data-types.md) | Root nodes of the document tree ||
|| **items[]**
[`object`](../../../data-types.md) | Tree document object ||
|| **id**
[`integer`](../../../data-types.md) | Document identifier ||
|| **collectionId**
[`integer`](../../../data-types.md) | Knowledge base identifier ||
|| **parentId**
[`integer`](../../../data-types.md) | Identifier of the parent document or `null` for the root page ||
|| **title**
[`string`](../../../data-types.md) | Document title ||
|| **position**
[`integer`](../../../data-types.md) | Position of the document among neighboring pages ||
|| **children**
[`array`](../../../data-types.md) | Child pages of the document ||
|| **truncated**
[`boolean`](../../../data-types.md) | The value of `true`, if the tree exceeded the internal limit `TREE_MAX_NODES = 5000` and was truncated by root nodes.

Exception — the first root document. If it alone exceeds the internal limit `TREE_MAX_NODES`, the method will return its initial part in breadth-first order so that the response is not empty. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error during request object validation",
        "validation": [
            {
                "message": "Mandatory field `collectionId` is missing",
                "field": "collectionId"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `collectionId` | Required field `collectionId` is not specified | Add `collectionId` to the request body ||
|| `collectionId` | Field `collectionId` requires data type `#TYPE#` for such a request | Ensure the provided value is of the correct type ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user does not have access to the Knowledge Base module or the permissions to view the specified knowledge base ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `collectionId` | Knowledge base not found | Please check that the knowledge base exists and is accessible to the user ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-document-get.md)
- [{#T}](./note-document-search-list.md)
- [{#T}](./index.md)