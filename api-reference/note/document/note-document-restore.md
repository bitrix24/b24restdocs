# Restore Document from Recycle Bin note.document.restore

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`note`](../../scopes/permissions.md)
>
> Who can execute the method: a Bitrix24 administrator or a user with access to the Knowledge base module and "Edit" permissions for the document's knowledge base. If the document's knowledge base has been permanently deleted, the document can also be restored by the user who deleted it or by its author

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `note.document.restore` method restores a document from the Recycle Bin together with the child pages that were deleted along with it.

{% note info "" %}

If the document's knowledge base is in the Recycle Bin, the document cannot be restored separately. Restore the entire knowledge base using the [note.collection.restore](../collection/note-collection-restore.md) method.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the document in the Recycle Bin.

The identifier can be obtained using the [note.document.list](./note-document-list.md) method with the `[["state","=","trashed"]]` filter.

An archived document is restored using the [note.document.unarchive](./note-document-unarchive.md) method ||
|| **collectionId**
[`integer`](../../data-types.md) | Identifier of the knowledge base to restore the document to. Requires "Edit" permissions for this knowledge base. A document cannot be restored to an archived knowledge base.

Required if the document's knowledge base has been permanently deleted. Without it, the method returns the `NOTE_RESTORE_TARGET_REQUIRED` error.

The identifier can be obtained using the [note.collection.list](../collection/note-collection-list.md) method.

Default: the knowledge base the document was deleted from. The document is placed under its former parent document if that parent is active ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.document.restore`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":133}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.document.restore
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":133,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.document.restore
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentRestoreResult = {
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
      const response = await $b24.actions.v3.call.make<DocumentRestoreResult>({
        method: 'note.document.restore',
        params: {
          id: 133,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Document restored:', result.item.id, result.item.state)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function restoreDocument() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'note.document.restore',
            params: {
              id: 133,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Document restored:', result.item.id, result.item.state)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', restoreDocument)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.note.document.restore(
            bitrix_id=133,
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
                'note.document.restore',
                [
                    'id' => 133,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'note.document.restore',
        {
            id: 133,
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
        'note.document.restore',
        [
            'id' => 133,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "note.document.restore", b24.Params{
    	"id": 133,
    })
    if err != nil {
    	return fmt.Errorf("note.document.restore: %w", err)
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
    	State        string `json:"state"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.CollectionID, item.State)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 133,
            "collectionId": 47,
            "parentId": 131,
            "title": "Child Document",
            "markdown": "",
            "position": 1000,
            "createdBy": 1,
            "updatedBy": 1,
            "createdAt": "2026-10-21T15:33:07+03:00",
            "updatedAt": "2026-10-21T15:37:32+03:00",
            "contentUpdatedAt": "2026-10-21T15:33:07+03:00",
            "state": "live",
            "isArchived": false,
            "isTrashed": false,
            "isOrphan": false
        }
    },
    "time": {
        "start": 1792586252,
        "finish": 1792586252.657717,
        "duration": 0.6577169895172119,
        "processing": 0,
        "date_start": "2026-10-21T15:37:32+03:00",
        "date_finish": "2026-10-21T15:37:32+03:00",
        "operating_reset_at": 1792586820,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the document data ||
|| **result.item**
[`object`](../../data-types.md) | The document as it was restored ||
|| **result.item.id**
[`integer`](../../data-types.md) | Document identifier ||
|| **result.item.collectionId**
[`integer`](../../data-types.md) or `null` | Identifier of the knowledge base the document was restored to ||
|| **result.item.parentId**
[`integer`](../../data-types.md) or `null` | Parent document identifier. `null` for a top-level document ||
|| **result.item.title**
[`string`](../../data-types.md) | Document title ||
|| **result.item.markdown**
[`string`](../../data-types.md) | Document content in Markdown ||
|| **result.item.position**
[`integer`](../../data-types.md) | Sort weight among sibling documents ||
|| **result.item.createdBy**
[`integer`](../../data-types.md) | Document author identifier ||
|| **result.item.updatedBy**
[`integer`](../../data-types.md) | Identifier of the user who last modified the document record ||
|| **result.item.createdAt**
[`datetime`](../../data-types.md) | Creation date and time in ISO 8601 format with a timezone offset ||
|| **result.item.updatedAt**
[`datetime`](../../data-types.md) | Date and time the document record was modified ||
|| **result.item.contentUpdatedAt**
[`datetime`](../../data-types.md) or `null` | Date and time the Markdown representation of the text was generated ||
|| **result.item.state**
[`string`](../../data-types.md) | Document state. After restoration — `live` ||
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
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 133 was not found."
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

#### Target Knowledge Base Error

Error Code: `NOTE_RESTORE_TARGET_REQUIRED`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `collectionId` | The original knowledge base has been deleted. Specify the knowledge base to restore the article to | Pass the identifier of an active knowledge base in `collectionId` ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user does not have access to the Knowledge Base module or permission to restore the document ||
|| `collectionId` | Access denied | The target knowledge base is in the archive, or the user does not have "Edit" permissions for it ||
|#

#### Access Escalation Error

Error Code: `NOTE_MOVE_ACCESS_ESCALATION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Moving the article will make it available to users who are currently denied access. Moderator permissions for the target space are required | The restore location is open to a wider group of users. The restoration must be performed by a user with "Administration" permissions for the knowledge base ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID #ID# was not found | The document does not exist, is not available to the user, or is not in the Recycle Bin. If the document's knowledge base is in the Recycle Bin, restore it using the [note.collection.restore](../collection/note-collection-restore.md) method ||
|| `collectionId` | Entity with ID #ID# was not found | The target knowledge base does not exist, is not available to the user, or is in the Recycle Bin ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-document-delete.md)
- [{#T}](./note-document-unarchive.md)
- [{#T}](../collection/note-collection-restore.md)
- [{#T}](./index.md)
