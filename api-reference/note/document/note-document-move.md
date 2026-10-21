# Move a Document note.document.move

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`note`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module and "Edit" permissions for both the source and the target knowledge base

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `note.document.move` method moves a document together with its child pages to another knowledge base, under another parent document, or to another position among its sibling documents.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Document identifier.

The identifier can be obtained using the [note.document.list](./note-document-list.md) or [note.document.tree.list](./note-document-tree-list.md) method.

A document in the archive or the Recycle Bin cannot be moved. Restore it first using the [note.document.unarchive](./note-document-unarchive.md) or [note.document.restore](./note-document-restore.md) method ||
|| **collectionId**
[`integer`](../../data-types.md) | Identifier of the target knowledge base. The knowledge base must be active: a document cannot be moved to an archived knowledge base.

The identifier can be obtained using the [note.collection.list](../collection/note-collection-list.md) method.

Default: the document's current knowledge base ||
|| **parentId**
[`integer`](../../data-types.md) | Identifier of the parent document in the target knowledge base.

The identifier can be obtained using the [note.document.tree.list](./note-document-tree-list.md) method.

If the parameter is not passed, the document becomes a top-level document. To change the order among child pages, pass the `parentId` of the current parent ||
|| **position**
[`integer`](../../data-types.md) | Sort weight among sibling documents. Documents with a higher weight are placed higher. To place a document between two siblings, pass a number between their `position` values.

The retained weight is recalculated based on the sibling documents and may differ from the passed value. The actual value is returned in the response.

Default: the document is placed last among its siblings ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.document.move`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":127,"collectionId":47,"parentId":125,"position":1500}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.document.move
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":127,"collectionId":47,"parentId":125,"position":1500,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.document.move
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentMoveResult = {
      item: {
        id: number
        collectionId: number | null
        parentId: number | null
        title: string
        markdown: string
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
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentMoveResult>({
        method: 'note.document.move',
        params: {
          id: 127,
          collectionId: 47,
          parentId: 125,
          position: 1500,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Document moved:', result.item.parentId, result.item.position)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function moveDocument() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'note.document.move',
            params: {
              id: 127,
              collectionId: 47,
              parentId: 125,
              position: 1500,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Document moved:', result.item.parentId, result.item.position)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', moveDocument)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.note.document.move(
            bitrix_id=127,
            collection_id=47,
            parent_id=125,
            position=1500,
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
                'note.document.move',
                [
                    'id' => 127,
                    'collectionId' => 47,
                    'parentId' => 125,
                    'position' => 1500,
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
        'note.document.move',
        {
            id: 127,
            collectionId: 47,
            parentId: 125,
            position: 1500,
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
        'note.document.move',
        [
            'id' => 127,
            'collectionId' => 47,
            'parentId' => 125,
            'position' => 1500,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "note.document.move", b24.Params{
    	"id":           127,
    	"collectionId": 47,
    	"parentId":     125,
    	"position":     1500,
    })
    if err != nil {
    	return fmt.Errorf("note.document.move: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID           b24.ID `json:"id"`
    	CollectionID b24.ID `json:"collectionId"`
    	ParentID     b24.ID `json:"parentId"`
    	Position     int    `json:"position"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.ParentID, item.Position)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 127,
            "collectionId": 47,
            "parentId": 125,
            "title": "VPN Guide",
            "markdown": "# VPN Guide\n\nConnection setup.",
            "position": 2000,
            "createdBy": 1,
            "updatedBy": 1,
            "createdAt": "2026-10-21T15:30:05+03:00",
            "updatedAt": "2026-10-21T15:31:29+03:00",
            "contentUpdatedAt": "2026-10-21T15:30:05+03:00",
            "state": "live",
            "isArchived": false,
            "isTrashed": false,
            "isOrphan": false
        }
    },
    "time": {
        "start": 1792585889,
        "finish": 1792585889.7807,
        "duration": 0.7806999683380127,
        "processing": 0,
        "date_start": "2026-10-21T15:31:29+03:00",
        "date_finish": "2026-10-21T15:31:29+03:00",
        "operating_reset_at": 1792586460,
        "operating": 0
    }
}
```

In this example, `position` = `1500` was passed, but the weight `2000` was retained: the value was recalculated based on the sibling documents.

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the document data ||
|| **result.item**
[`object`](../../data-types.md) | The document as it was retained after the move ||
|| **result.item.id**
[`integer`](../../data-types.md) | Document identifier ||
|| **result.item.collectionId**
[`integer`](../../data-types.md) or `null` | Knowledge base identifier. `null` if the user has access only to the document itself, not to the knowledge base ||
|| **result.item.parentId**
[`integer`](../../data-types.md) or `null` | Parent document identifier. `null` for a top-level document ||
|| **result.item.title**
[`string`](../../data-types.md) | Document title ||
|| **result.item.markdown**
[`string`](../../data-types.md) | Document content in Markdown ||
|| **result.item.position**
[`integer`](../../data-types.md) | Retained sort weight among sibling documents ||
|| **result.item.createdBy**
[`integer`](../../data-types.md) | Document author identifier ||
|| **result.item.updatedBy**
[`integer`](../../data-types.md) | Identifier of the user who last modified the document record ||
|| **result.item.createdAt**
[`datetime`](../../data-types.md) | Creation date and time in ISO 8601 format with a timezone offset ||
|| **result.item.updatedAt**
[`datetime`](../../data-types.md) | Date and time the document record was modified. Moving a document updates this field ||
|| **result.item.contentUpdatedAt**
[`datetime`](../../data-types.md) or `null` | Date and time the Markdown representation of the text was generated ||
|| **result.item.state**
[`string`](../../data-types.md) | Document state: `live` — active, `archived` — in the archive, `trashed` — in the Recycle Bin ||
|| **result.item.isArchived**
[`boolean`](../../data-types.md) | Deprecated field. Use `state`. `true` — the document is in the archive ||
|| **result.item.isTrashed**
[`boolean`](../../data-types.md) | Deprecated field. Use `state`. `true` — the document is in the Recycle Bin ||
|| **result.item.isOrphan**
[`boolean`](../../data-types.md) | Deprecated field. `true` — the document is in the Recycle Bin and its knowledge base has been deleted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "NOTE_INVALID_PARENT",
        "message": "Parent article is not part of the specified space."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field "id" is missing | Add `id` to the request body ||
|| `id` | This request requires data type "int" in field "id" | Pass the identifier as a number ||
|#

#### Parent Document Error

Error Code: `NOTE_INVALID_PARENT`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `parentId` | Parent article is not part of the specified space | Pass the `parentId` of a document from the target knowledge base, or specify the parent's knowledge base in `collectionId` ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user does not have access to the Knowledge Base module or "Edit" permissions for the source or target knowledge base ||
|| `collectionId` | Access denied | The target knowledge base is in the archive. Restore it using the [note.collection.unarchive](../collection/note-collection-unarchive.md) method ||
|#

#### Access Escalation Error

Error Code: `NOTE_MOVE_ACCESS_ESCALATION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Moving the article will make it available to users who are currently denied access. Moderator permissions for the target space are required | The target branch is open to a wider group of users. The move must be performed by a user with "Administration" permissions for the knowledge base ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID #ID# was not found | The document does not exist, is not available to the user, or is in the archive or the Recycle Bin. The root document of a knowledge base, which stores its description, cannot be moved ||
|| `collectionId` | Entity with ID #ID# was not found | The target knowledge base does not exist, is not available to the user, or is in the Recycle Bin ||
|| `parentId` | Entity with ID #ID# was not found | The parent document does not exist or is not available to the user ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-document-tree-list.md)
- [{#T}](./note-document-update.md)
- [{#T}](../collection/note-collection-move.md)
- [{#T}](./index.md)
