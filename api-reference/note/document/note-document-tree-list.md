# Get Document Tree note.document.tree.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`note`](../../scopes/permissions.md)
>
> Who can execute the method: a User with access to the Knowledge base module and "View" permissions for the document knowledge base

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `note.document.tree.list` method returns the document tree of a single knowledge base.

{% note info "" %}

Archived documents are not included in the response.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **collectionId***
[`integer`](../../data-types.md) | Knowledge base identifier.

The identifier can be obtained using the [note.collection.list](../collection/note-collection-list.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

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
      truncated?: boolean
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
        console.info('Tree roots:', result.items.length)
        if (result.truncated !== undefined) {
          console.info('Tree truncated:', result.truncated)
        }
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
          console.info('Tree roots:', result.items.length)
          if (result.truncated !== undefined) {
            console.info('Tree truncated:', result.truncated)
          }
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getDocumentTree)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.note.document.tree.list(
            collection_id=123,
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "note.document.tree.list", b24.Params{
    	"collectionId": 123,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("note.document.tree.list: %w", err)
    }

    type TreeNode struct {
        ID           int        `json:"id"`
        CollectionID int        `json:"collectionId"`
        ParentID     *int       `json:"parentId"`
        Title        string     `json:"title"`
        Position     int        `json:"position"`
        Children     []TreeNode `json:"children"`
    }
    var result struct {
        Items     []TreeNode `json:"items"`
        Truncated *bool      `json:"truncated"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("Root documents:", len(result.Items))
    if result.Truncated != nil {
        fmt.Println("Tree truncated:", *result.Truncated)
    }
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
[`object`](../../data-types.md) | Object with the document tree ||
|| **result.items**
[`array`](../../data-types.md) | Root [tree nodes](#tree-node) of the documents ||
|| **result.items[]**
[`object`](../../data-types.md) | Root [tree node](#tree-node) of the documents ||
|| **result.truncated**
[`boolean`](../../data-types.md) | `true` if the tree exceeded the internal limit `TREE_MAX_NODES = 5000` and was truncated at root nodes.

The first root document is an exception. If it alone exceeds `TREE_MAX_NODES`, the method returns its initial portion in breadth-first order so that the response is not empty.

This field may be absent from the response. In that case, the response does not indicate whether the tree was truncated ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Tree Node {#tree-node}

Each element of `result.items` and the nested `children` array has the same structure.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Document identifier ||
|| **collectionId**
[`integer`](../../data-types.md) | Knowledge base identifier ||
|| **parentId**
[`integer`](../../data-types.md) | Identifier of the parent document or `null` for the root page ||
|| **title**
[`string`](../../data-types.md) | Document title ||
|| **position**
[`integer`](../../data-types.md) | Position of the document among neighboring pages ||
|| **children**
[`array`](../../data-types.md) | Child [tree nodes](#tree-node). An empty array if the document has no children ||
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

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

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

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-document-get.md)
- [{#T}](./note-document-search-list.md)
- [{#T}](./index.md)
