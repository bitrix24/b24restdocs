# Upload a File to Document note.file.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`note`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module and "Edit" permissions for the document Knowledge base

The `note.file.add` method uploads a file to a document and returns a file object.

{% note info "" %}

The method does not automatically insert the attachment into the document content. It only saves the file and links it to the document.

{% endnote %}

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **documentId***
[`integer`](../../../data-types.md) | Identifier of the document to which the file should be attached.

The identifier can be obtained using the [note.document.tree.list](../document/note-document-tree-list.md) method ||
|| **fileName***
[`string`](../../../data-types.md) | File name with extension.

Allowed extensions: `png`, `jpg`, `jpeg`, `gif`, `webp`, `svg`, `pdf`, `txt`, `md`, `csv`, `doc`, `docx`, `xls`, `xlsx`, `ppt`, `pptx`, `mp4`, `webm`, `mov` ||
|| **fileContent***
[`string`](../../../data-types.md) | Binary file content in [Base64](../../../files/how-to-upload-files.md) encoding.

The maximum file size depends on the `main.max_file_size` setting in Bitrix24. If it is not set, the `25 MiB (25 * 1024 * 1024 bytes)` limit is used ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.file.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"documentId":77,"fileName":"diagram.png","fileContent":"iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mNkYAAAAAYAAjCB0C8AAAAASUVORK5CYII="}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.file.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"documentId":77,"fileName":"diagram.png","fileContent":"iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mNkYAAAAAYAAjCB0C8AAAAASUVORK5CYII=","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.file.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FileAddResult = {
      item: {
        id: number
        documentId: number
        name: string
        size: number
        mimeType: string
        assetType: string
        assetMarkdown: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<FileAddResult>({
        method: 'note.file.add',
        params: {
          documentId: 77,
          fileName: 'diagram.png',
          fileContent: 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mNkYAAAAAYAAjCB0C8AAAAASUVORK5CYII=',
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('File uploaded:', result.item.id, result.item.assetMarkdown)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function addFile() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'note.file.add',
            params: {
              documentId: 77,
              fileName: 'diagram.png',
              fileContent: 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mNkYAAAAAYAAjCB0C8AAAAASUVORK5CYII=',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('File uploaded:', result.item.id, result.item.assetMarkdown)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addFile)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'note.file.add',
                [
                    'documentId' => 77,
                    'fileName' => 'diagram.png',
                    'fileContent' => 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mNkYAAAAAYAAjCB0C8AAAAASUVORK5CYII=',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error uploading file: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'note.file.add',
        {
            documentId: 77,
            fileName: 'diagram.png',
            fileContent: 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mNkYAAAAAYAAjCB0C8AAAAASUVORK5CYII='
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
        'note.file.add',
        [
            'documentId' => 77,
            'fileName' => 'diagram.png',
            'fileContent' => 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mNkYAAAAAYAAjCB0C8AAAAASUVORK5CYII='
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
            "id": 5001,
            "documentId": 77,
            "name": "diagram.png",
            "size": 6321,
            "mimeType": "image/png",
            "assetType": "image",
            "assetMarkdown": "[[image fileId=5001]]"
        }
    },
    "time": {
        "start": 1780392000,
        "finish": 1780392000.284521,
        "duration": 0.28452086448669434,
        "processing": 0.2413930892944336,
        "date_start": "2026-06-16T12:20:00+03:00",
        "date_finish": "2026-06-16T12:20:00+03:00",
        "operating_reset_at": 1780392600,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the file upload result ||
|| **item**
[`object`](../../../data-types.md) | Object of the uploaded file ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the uploaded file ||
|| **documentId**
[`integer`](../../../data-types.md) | Identifier of the document to which the file is attached ||
|| **name**
[`string`](../../../data-types.md) | File name ||
|| **size**
[`integer`](../../../data-types.md) | File size in bytes ||
|| **mimeType**
[`string`](../../../data-types.md) | File MIME type ||
|| **assetType**
[`string`](../../../data-types.md) | Attachment type for the Markdown block ||
|| **assetMarkdown**
[`string`](../../../data-types.md) | Ready-to-use Markdown block for inserting the file into a document via [note.document.update](../document/note-document-update.md) ||
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
                "message": "invalid base64 payload",
                "field": "fileContent"
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
|| `documentId`
`fileName`
`fileContent` | Required field `#FIELD#` is not specified | Add the specified field to the request body ||
|| `documentId`
`fileName`
`fileContent` | The field `#FIELD#` requires data type `#TYPE#` for this request | Ensure the provided value is of the correct type ||
|| `fileContent` | Incorrect Base64 content provided | Please ensure that `fileContent` contains a non-empty Base64 string without corrupted characters ||
|| `fileContent` | Failed to save the file | Retry the request later or check the correctness of the provided data ||
|#

Error Code: `NOTE_FILE_TOO_LARGE`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fileContent` | File size exceeds the allowed value | Reduce the file size or change the `main.max_file_size` setting in Bitrix24 ||
|#

Error Code: `NOTE_FILE_TYPE_NOT_ALLOWED`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fileName` | File type is not supported | Use a file with an allowed extension ||
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
|| `documentId` | Document not found | Please ensure that the document exists, is not archived, and is not in the trash ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-file-get.md)
- [{#T}](../document/note-document-update.md)
- [{#T}](./index.md)