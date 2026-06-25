# Create Document note.document.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`note`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module and "Edit" permissions for the required knowledge base

The `note.document.add` method creates a new document in the knowledge base and returns its object.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with the fields of the new document. [Object structure description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **collectionId***
[`integer`](../../../data-types.md) | Knowledge base identifier.

The identifier can be obtained using the [note.collection.list](../collection/note-collection-list.md) method. ||
|| **title***
[`string`](../../../data-types.md) | Document title.

The document title must not exceed 255 characters. ||
|| **parentId**
[`integer`](../../../data-types.md) | Parent document identifier.

The identifier can be obtained using the [note.document.tree.list](./note-document-tree-list.md) method.

Use this if you need to create a nested document. The parent document must belong to the same knowledge base as `collectionId`. ||
|| **markdown**
[`string`](../../../data-types.md) | Initial document content in Markdown.

Maximum size: `1 048 576` bytes. ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.document.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"collectionId":42,"title":"Chapter 1","parentId":10,"markdown":"# Chapter 1\N\nDocument Text"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.document.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"collectionId":42,"title":"Chapter 1","parentId":10,"markdown":"# Chapter 1\N\nDocument Text"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.document.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type DocumentAddResult = {
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
      const response = await $b24.actions.v3.call.make<DocumentAddResult>({
        method: 'note.document.add',
        params: {
          fields: {
            collectionId: 42,
            title: 'Chapter 1',
            parentId: 10,
            markdown: '# Chapter 1\N\nDocument Text',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Document created:', result.item.id, result.item.title)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function addDocument() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'note.document.add',
            params: {
              fields: {
                collectionId: 42,
                title: 'Chapter 1',
                parentId: 10,
                markdown: '# Chapter 1\N\nDocument Text',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Document created:', result.item.id, result.item.title)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addDocument)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'note.document.add',
                [
                    'fields' => [
                        'collectionId' => 42,
                        'title' => 'Chapter 1',
                        'parentId' => 10,
                        'markdown' => "# Chapter 1\N\nDocument Text",
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating document: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'note.document.add',
        {
            fields: {
                collectionId: 42,
                title: 'Chapter 1',
                parentId: 10,
                markdown: '# Chapter 1\N\nDocument Text'
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
        'note.document.add',
        [
            'fields' => [
                'collectionId' => 42,
                'title' => 'Chapter 1',
                'parentId' => 10,
                'markdown' => "# Chapter 1\N\nDocument Text"
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
        "item": {
            "id": 77,
            "collectionId": 42,
            "parentId": 10,
            "title": "Chapter 1",
            "markdown": "

# Chapter 1\N\nText in Markdown...",
            "position": 5,
            "createdBy": 1,
            "updatedBy": 1,
            "createdAt": "2026-04-20T12:00:00Z",
            "updatedAt": "2026-04-20T12:00:00Z"
        }
    },
    "time": {
        "start": 1780390800,
        "finish": 1780390800.214321,
        "duration": 0.21432113647460938,
        "processing": 0.17321419715881348,
        "date_start": "2026-06-16T12:00:00+03:00",
        "date_finish": "2026-06-16T12:00:00+03:00",
        "operating_reset_at": 1780391400,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the document creation result. ||
|| **item**
[`object`](../../../data-types.md) | Created document object. ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the created document ||
|| **collectionId**
[`integer`](../../../data-types.md) | Knowledge base identifier. ||
|| **parentId**
[`integer`](../../../data-types.md) | Parent document identifier or `null`. ||
|| **title**
[`string`](../../../data-types.md) | Document title. ||
|| **markdown**
[`string`](../../../data-types.md) | Document content in Markdown. ||
|| **position**
[`integer`](../../../data-types.md) | Document position among neighboring pages. ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the document author ||
|| **updatedBy**
[`integer`](../../../data-types.md) | Last document editor identifier. ||
|| **createdAt**
[`datetime`](../../../data-types.md) | Document creation date and time in UTC. ||
|| **updatedAt**
[`datetime`](../../../data-types.md) | Document last modification date and time in UTC. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION",
        "message": "Object validation error",
        "validation": [
            {
                "message": "Mandatory field \"title\" is not filled",
                "field": "title"
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
|| `fields` | Required field `fields` is not specified. | Add the `fields` object to the request body. ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `collectionId`
`title` | Required field `#FIELD#` is not filled. | Add the specified field to the `fields` object. ||
|| `collectionId`
`title`
`parentId`
`markdown` | The field `#FIELD#` requires data type `#TYPE#` for this request. | Ensure the provided value is of the correct type ||
|| `title` | Document title cannot be empty. | Pass a non-empty string in the `fields.title` field. ||
|| `title` | Document title must not exceed 255 characters. | Shorten the `fields.title` value. ||
|#

Error Code: `NOTE_MARKDOWN_TOO_LARGE`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `markdown` | Document content is too large. The maximum allowed size is `1 048 576` bytes. | Shorten the `fields.markdown` content. ||
|#

Error Code: `NOTE_INVALID_PARENT`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `parentId` | The parent document does not belong to the specified collection. | Specify the `fields.parentId` of the document from the same knowledge base as `fields.collectionId`, or do not pass `fields.parentId`. ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied. | The user does not have access to the Knowledge Base module or the permissions to create documents in the specified knowledge base. ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `collectionId` | Knowledge base not found. | Please check that the knowledge base exists and is accessible to the user. ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-document-update.md)
- [{#T}](./note-document-archive.md)
- [{#T}](./note-document-delete.md)
- [{#T}](./index.md)