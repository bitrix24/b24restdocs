# Update Document note.document.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`note`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module and "Edit" permissions for the required document or document knowledge base

The `note.document.update` method updates the heading and/or the content of a document.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Document identifier.

The identifier can be obtained using the [note.document.tree.list](./note-document-tree-list.md) method. ||
|| **fields***
[`object`](../../../data-types.md) | An object with the fields that need to be changed. [Object structure description](#fields) ||
|| **overwrite**
[`boolean`](../../../data-types.md) | Determines whether to force overwrite the document content if it has unsaved changes from a collaborative editor.

Possible values:

- `true` — overwrite the document content
- `false` — do not overwrite the content if there are unsaved changes

Default: `false`

Use this if you are passing `fields.markdown` ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../../data-types.md) | New document title.

The document title must not exceed 255 characters.

Pass `fields.title`, `fields.markdown`, or both fields at once. ||
|| **markdown**
[`string`](../../../data-types.md) | New document content in Markdown.

To add a file to the document, insert `assetMarkdown`, obtained via the [note.file.get](../file/note-file-get.md) or [note.file.add](../file/note-file-add.md) method, as a separate line at the beginning of the line, without a prefix or suffix in the same line.

Maximum size: `1 048 576` bytes.

Pass `fields.title`, `fields.markdown`, or both fields at once. ||
|#

{% note info "" %}

If the document is currently being edited in the interface, the method will return error `NOTE_DOCUMENT_HAS_UNSAVED_CHANGES`. To force overwrite the content, repeat the request with `overwrite=true`. All active editors will be sent a signal via a P&P event to reload the current content.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.document.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":77,"fields":{"title":"Chapter 1 (updated)","markdown":"# Chapter 1\N\nUpdated Text"},"Overwrite":False}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.document.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":77,"fields":{"title":"Chapter 1 (updated)","markdown":"# Chapter 1\N\nUpdated Text"},"Overwrite":False,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.document.update
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentUpdateResult = {
      item: {
        id: number
        collectionId: number | null
        parentId: number | null
        title: string
        markdown: string
        position: number
        createdBy: number
        updatedBy: number
        createdAt: string
        updatedAt: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<DocumentUpdateResult>({
        method: 'note.document.update',
        params: {
          id: 77,
          fields: {
            title: 'Chapter 1 (updated)',
            markdown: '# Chapter 1\N\nUpdated Text',
          },
          overwrite: false,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Document updated:', result.item.id, result.item.title)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function updateDocument() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'note.document.update',
            params: {
              id: 77,
              fields: {
                title: 'Chapter 1 (updated)',
                markdown: '# Chapter 1\N\nUpdated Text',
              },
              overwrite: false,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Document updated:', result.item.id, result.item.title)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateDocument)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'note.document.update',
                [
                    'id' => 77,
                    'fields' => [
                        'title' => 'Chapter 1 (updated)',
                        'markdown' => "# Chapter 1\N\nUpdated Text",
                    ],
                    'overwrite' => false,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating document: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'note.document.update',
        {
            id: 77,
            fields: {
                title: 'Chapter 1 (updated)',
                markdown: '# Chapter 1\N\nUpdated Text'
            },
            overwrite: false
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
        'note.document.update',
        [
            'id' => 77,
            'fields' => [
                'title' => 'Chapter 1 (updated)',
                'markdown' => "# Chapter 1\N\nUpdated Text"
            ],
            'overwrite' => false
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
        "item": {
            "id": 77,
            "collectionId": 42,
            "parentId": 10,
            "title": "Chapter 1 (updated)",
            "markdown": "# Chapter 1\N\nUpdated Text",
            "position": 5,
            "createdBy": 1,
            "updatedBy": 1,
            "createdAt": "2026-04-20T12:00:00Z",
            "updatedAt": "2026-04-21T09:15:30Z"
        }
    },
    "time": {
        "start": 1780391100,
        "finish": 1780391100.266341,
        "duration": 0.266340970993042,
        "processing": 0.22421908378601074,
        "date_start": "2026-06-16T12:05:00+03:00",
        "date_finish": "2026-06-16T12:05:00+03:00",
        "operating_reset_at": 1780391700,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the document update result ||
|| **item**
[`object`](../../../data-types.md) | Document object after the update ||
|| **id**
[`integer`](../../../data-types.md) | Document identifier ||
|| **collectionId**
[`integer`](../../../data-types.md) | Knowledge base identifier or `null` if the document is available via direct document access ||
|| **parentId**
[`integer`](../../../data-types.md) | Parent document identifier or `null` ||
|| **title**
[`string`](../../../data-types.md) | Document title ||
|| **markdown**
[`string`](../../../data-types.md) | Document content in Markdown ||
|| **position**
[`integer`](../../../data-types.md) | Document position among neighboring pages ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the document author ||
|| **updatedBy**
[`integer`](../../../data-types.md) | Identifier of the last document editor ||
|| **createdAt**
[`datetime`](../../../data-types.md) | Document creation date and time in UTC ||
|| **updatedAt**
[`datetime`](../../../data-types.md) | Document last modified date and time in UTC ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "NOTE_EMPTY_UPDATE",
        "message": "The request does not contain any modifiable fields"
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id`
`fields` | Required field `#FIELD#` is not specified | Add the specified field to the request body ||
|| `id`
`overwrite` | Field `#FIELD#` requires data type `#TYPE#` for this request | Ensure the provided value is of the correct type ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `title`
`markdown` | Field `#FIELD#` requires data type `#TYPE#` for this request | Ensure the provided value is of the correct type ||
|| `title` | Document title cannot be empty | Pass a non-empty string in the `fields.title` field ||
|| `title` | Document title must not exceed 255 characters | Shorten the value of `fields.title` ||
|#

Error Code: `NOTE_EMPTY_UPDATE`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields.title`
`fields.markdown` | The request does not contain any changeable fields | Pass `fields.title`, `fields.markdown`, or both fields at once ||
|#

Error Code: `NOTE_MARKDOWN_TOO_LARGE`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `markdown` | Document content is too large. The maximum allowed size is `1 048 576` bytes | Shorten the content of `fields.markdown` ||
|#

#### Conflict Error

Error Code: `NOTE_DOCUMENT_HAS_UNSAVED_CHANGES`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `overwrite` | The document has unsaved changes. Pass the `overwrite=true` parameter to overwrite the content | Retry the request with `overwrite=true` if you need to force overwrite the document content ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | The user does not have access to the Knowledge Base module or permissions to edit the document ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Document not found | Check that the document exists, is not archived, and is not in the trash ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-document-add.md)
- [{#T}](./note-document-archive.md)
- [{#T}](./note-document-delete.md)
- [{#T}](./index.md)