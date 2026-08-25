# Get Document File Data note.file.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`note`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the Knowledge base module and "View" permissions for the document's knowledge base

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `note.file.get` method returns file metadata and a ready-to-use Markdown block to insert the attachment into a document.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | File identifier.

The file identifier can be obtained in the response of the [note.file.add](./note-file-add.md) method. ||
|| **documentId***
[`integer`](../../data-types.md) | Document identifier to which the file is attached.

Use the same document identifier that you passed to the [note.file.add](./note-file-add.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/note.file.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":5001,"documentId":77}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/note.file.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":5001,"documentId":77,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/note.file.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FileGetResult = {
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
      const response = await $b24.actions.v3.call.make<FileGetResult>({
        method: 'note.file.get',
        params: {
          id: 5001,
          documentId: 77,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('File info:', result.item.assetMarkdown)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getFile() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'note.file.get',
            params: {
              id: 5001,
              documentId: 77,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('File info:', result.item.assetMarkdown)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getFile)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.note.file.get(
            bitrix_id=5001,
            document_id=77,
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
                'note.file.get',
                [
                    'id' => 5001,
                    'documentId' => 77,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting file info: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'note.file.get',
        {
            id: 5001,
            documentId: 77
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
        'note.file.get',
        [
            'id' => 5001,
            'documentId' => 77
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "note.file.get", b24.Params{
    	"id":         5001,
    	"documentId": 77,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("note.file.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID         b24.ID `json:"id"`
    	DocumentID b24.ID `json:"documentId"`
    	Name       string `json:"name"`
    	Size       int    `json:"size"`
    	MimeType   string `json:"mimeType"`
    	AssetType  string `json:"assetType"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.DocumentID)
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
        "start": 1780392300,
        "finish": 1780392300.194822,
        "duration": 0.19482207298278809,
        "processing": 0.15441107749938965,
        "date_start": "2026-06-16T12:25:00+03:00",
        "date_finish": "2026-06-16T12:25:00+03:00",
        "operating_reset_at": 1780392900,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object containing the result of the file data retrieval. ||
|| **item**
[`object`](../../data-types.md) | Object containing file metadata. ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the file ||
|| **documentId**
[`integer`](../../data-types.md) | Document identifier to which the file is attached.

This is the same document identifier that you passed to the [note.file.add](./note-file-add.md) method. ||
|| **name**
[`string`](../../data-types.md) | File name. ||
|| **size**
[`integer`](../../data-types.md) | File size in bytes ||
|| **mimeType**
[`string`](../../data-types.md) | File MIME type. ||
|| **assetType**
[`string`](../../data-types.md) | Attachment type for Markdown.

Possible values:

- `image` — image
- `video` — video
- `file` — regular file ||
|| **assetMarkdown**
[`string`](../../data-types.md) | Ready-to-use Markdown block for insertion into a document in `[[<assetType> fileId=<id>]]` format.

To make the attachment appear in the document, pass this block to `markdown` via [note.document.update](../document/note-document-update.md).

Before inserting an image or video, you can extend the block with scale and alignment parameters. For details, see the `markdown` field description in [note.document.update](../document/note-document-update.md). ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object",
        "validation": [
            {
                "message": "Mandatory field `id` is missing",
                "field": "id"
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
|| `id`
`documentId` | Required field `#FIELD#` is not specified. | Add the specified field to the request body ||
|| `id`
`documentId` | Field `#FIELD#` requires data type `#TYPE#` for such a request. | Ensure the provided value is of the correct type ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `documentId` | Document not found. | Please check that the document exists, is not archived, is not in the trash, and is accessible to the user. ||
|| `id` | File not found. | Please check that the file exists and is attached to the specified document. ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./note-file-add.md)
- [{#T}](../document/note-document-update.md)
- [{#T}](./index.md)
