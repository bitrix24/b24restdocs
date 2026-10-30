# Upload File to Chat im.v2.File.upload

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`im`](../../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the chat

The method `im.v2.File.upload` uploads a file to the chat on behalf of the current user. It combines three steps of the deprecated API into a single call: uploading the file to the Drive, attaching it to the chat, and sending a message.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **dialogId***
[`string`](../../../../data-types.md) | ID of the dialog. For group chats — `chat{chatId}`, for personal chats — `{userId}` ||
|| **fields***
[`object`](../../../../data-types.md) | Object with file and message parameters [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`Type` | **Description** ||
|| **name***
[`string`](../../../../data-types.md) | File name with extension ||
|| **content***
[`string`](../../../../data-types.md) | File content in [Base64](../../../../files/how-to-upload-files.md) encoding. The maximum file size after decoding is 100 MB ||
|| **message**
[`string`](../../../../data-types.md) | Text of the message sent along with the file ||
|#

{% note info "" %}

How to prepare the value for `fields.content`:

1. Read the file in binary format.
2. Encode the content in Base64.
3. Pass only the Base64 string, without the prefix `data:*/*;base64,`.

More details: [How to upload files](../../../../files/how-to-upload-files.md#how-to-encode-a-file-in-base64).

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"dialogId":"chat5","fields":{"name":"report.txt","content":"SGVsbG8gV29ybGQh","message":"Here is the report"}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.v2.File.upload
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"dialogId":"chat5","fields":{"name":"report.txt","content":"SGVsbG8gV29ybGQh","message":"Here is the report"},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.v2.File.upload
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Minimal shape of result; the API returns the full File object
    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FileUploadResult = {
      file: {
        id: number
        name: string
      }
      messageId: number
      chatId: number
      dialogId: string
    }

    try {
      const response = await $b24.actions.v2.call.make<FileUploadResult>({
        method: 'im.v2.File.upload',
        params: {
          dialogId: 'chat5',
          fields: {
            name: 'report.txt',
            content: 'SGVsbG8gV29ybGQh',
            message: 'Here is the report',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('File ID:', result.file.id, '| Message ID:', result.messageId)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function uploadFile() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.v2.File.upload',
            params: {
              dialogId: 'chat5',
              fields: {
                name: 'report.txt',
                content: 'SGVsbG8gV29ybGQh',
                message: 'Here is the report',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('File ID:', result.file.id, '| Message ID:', result.messageId)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', uploadFile)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.im.v2.file.upload(
            dialog_id="chat5",
            fields={
                "name": "report.txt",
                "content": "SGVsbG8gV29ybGQh",
                "message": "Here is the report",
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

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.v2.File.upload',
                [
                    'dialogId' => 'chat5',
                    'fields' => [
                        'name' => 'report.txt',
                        // for your own file: base64_encode(file_get_contents('/path/to/report.txt'))
                        'content' => 'SGVsbG8gV29ybGQh',
                        'message' => 'Here is the report',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'result: '. print_r($result, true);
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error: '. $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.v2.File.upload',
        {
            dialogId: 'chat5',
            fields: {
                name: 'report.txt',
                content: 'SGVsbG8gV29ybGQh',
                message: 'Here is the report',
            },
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.v2.File.upload',
        [
            'dialogId' => 'chat5',
            'fields' => [
                'name' => 'report.txt',
                // for your own file: base64_encode(file_get_contents('/path/to/report.txt'))
                'content' => 'SGVsbG8gV29ybGQh',
                'message' => 'Here is the report',
            ],
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: '. $result['error_description'];
    } else {
        echo 'File ID: '. $result['result']['file']['id'];
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "im.v2.File.upload", b24.Params{
    	"dialogId": "chat5",
    	"fields": b24.Params{
    		"name":    "report.txt",
    		"content": "SGVsbG8gV29ybGQh",
    		"message": "Here is the report",
    	},
    })
    if err != nil {
    	return fmt.Errorf("im.v2.File.upload: %w", err)
    }

    var item struct {
    	DialogID  string `json:"dialogId"`
    	ChatID    b24.ID `json:"chatId"`
    	MessageID b24.ID `json:"messageId"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.DialogID, item.ChatID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "file": {
            "id": 9817,
            "chatId": 4415,
            "date": "2026-08-11T15:28:50+02:00",
            "type": "file",
            "name": "report.txt",
            "extension": "txt",
            "size": 12,
            "image": false,
            "status": "done",
            "progress": 100,
            "authorId": 1,
            "authorName": "Klaus Weber",
            "urlPreview": "",
            "urlShow": "https://some-domain.bitrix24.com/bitrix/services/main/ajax.php?action=disk.api.file.download&SITE_ID=s1&humanRE=1&fileId=9817&exact=N&fileName=report.txt",
            "urlDownload": "https://some-domain.bitrix24.com/bitrix/services/main/ajax.php?action=disk.api.file.download&SITE_ID=s1&humanRE=1&fileId=9817&exact=N&fileName=report.txt",
            "viewerAttrs": {
                "viewer": "",
                "viewerType": "code",
                "src": "https://some-domain.bitrix24.com/bitrix/services/main/ajax.php?action=disk.api.file.download&SITE_ID=s1&humanRE=1&fileId=9817&exact=N&fileName=report.txt",
                "objectId": "9817",
                "viewerGroupBy": "4415",
                "imChatId": 4415,
                "title": "report.txt",
                "unifiedLink": "https://some-domain.bitrix24.com/file/5tqNA6utERA5MIWnKpFX",
                "actions": "[{\"type\":\"download\"}]"
            },
            "mediaUrl": {
                "preview": {
                    "250": ""
                }
            },
            "isTranscribable": false,
            "isVideoNote": false,
            "isVoiceNote": false
        },
        "messageId": 38655,
        "chatId": 4415,
        "dialogId": "chat4415"
    },
    "time": {
        "start": 1786451330,
        "finish": 1786451330.233862,
        "duration": 0.23386192321777344,
        "processing": 0,
        "date_start": "2026-08-11T15:28:50+02:00",
        "date_finish": "2026-08-11T15:28:50+02:00",
        "operating_reset_at": 1786451790,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | Result of the operation ||
|| **result.dialogId**
[`string`](../../../../data-types.md) | Identifier of the dialog ||
|| **result.chatId**
[`integer`](../../../../data-types.md) | Numeric identifier of the chat ||
|| **result.messageId**
[`integer`](../../../../data-types.md) | Identifier of the created message with the file ||
|| **result.file**
[`File`](../../entities.md#file) | Data of the uploaded file [(detailed description)](#file-object) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

#### File Object {#file-object}

#|
|| **Name**
`Type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the file on Drive ||
|| **chatId**
[`integer`](../../../../data-types.md) | Identifier of the chat ||
|| **date**
[`datetime`](../../../../data-types.md) | Date the file was uploaded ||
|| **type**
[`string`](../../../../data-types.md) | File type: `file`, `image`, `video`, `audio` ||
|| **name**
[`string`](../../../../data-types.md) | Name of the file ||
|| **extension**
[`string`](../../../../data-types.md) | File extension ||
|| **size**
[`integer`](../../../../data-types.md) | Size of the file in bytes ||
|| **image**
[```object | boolean```](../../../../data-types.md#compound-types) | `false` for a regular file. For an image — an object with the height and width, for example `{"height": 600, "width": 800}` ||
|| **status**
[`string`](../../../../data-types.md) | Upload status, for example `done` ||
|| **progress**
[`integer`](../../../../data-types.md) | Upload progress in percent ||
|| **authorId**
[`integer`](../../../../data-types.md) | Identifier of the user who uploaded the file ||
|| **authorName**
[`string`](../../../../data-types.md) | Name of the user who uploaded the file ||
|| **urlPreview**
[`string`](../../../../data-types.md) | Link to the file preview or an empty string ||
|| **urlShow**
[`string`](../../../../data-types.md) | Link to view the file ||
|| **urlDownload**
[`string`](../../../../data-types.md) | Link to download the file ||
|| **viewerAttrs**
[`object`](../../../../data-types.md) | Internal attributes for the built-in Bitrix24 viewer: the view type `viewerType`, the file address `src`, the file link `unifiedLink`, the actions `actions` as a JSON string, and others. Integrations do not need to parse them: to download the file, use `urlDownload` or the [im.v2.File.download](./file-download.md) method ||
|| **mediaUrl**
[`object`](../../../../data-types.md) | Links to previews by size. For an image, the `preview.250` key contains the preview address; for other files, an empty string ||
|| **isTranscribable**
[`boolean`](../../../../data-types.md) | Indicates a file that can be transcribed into text ||
|| **isVideoNote**
[`boolean`](../../../../data-types.md) | Indicates a video message ||
|| **isVoiceNote**
[`boolean`](../../../../data-types.md) | Indicates a voice message ||
|#

A complete description of all object fields can be found on the [Objects and Fields](../../entities.md) page.

## Error Handling

HTTP Status: **400**

```json
{
    "error": "FILE_TOO_LARGE",
    "error_description": "File size exceeds maximum allowed"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `FILE_EMPTY` | File name and content are required | The file name or content was not passed ||
|| `FILE_INVALID_CONTENT` | Invalid base64 content | The file content is not a Base64 string ||
|| `CHAT_NOT_FOUND` | CHAT_NOT_FOUND | The chat from `dialogId` was not found ||
|| `FILE_FOLDER_ERROR` | Could not get chat folder | Failed to retrieve the chat folder ||
|| `FILE_UPLOAD_FAILED` | File upload failed | Failed to store the file on Drive ||
|| `FILE_SEND_FAILED` | Failed to send file message | The file was stored, but the message containing it was not sent ||
|| `FILE_TOO_LARGE` | File size exceeds maximum allowed | File size exceeds 100 MB ||
|| `ACCESS_DENIED` | ACCESS_DENIED | The user is not a member of the chat or cannot send messages to it ||
|| `100` | `Could not find value for parameter {fields}` | The `fields` object was not passed. Without `dialogId`, the error text is `Could not find value for parameter {chatId}` ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API imbot.v2 Change Log](../../change-log.md)
- [{#T}](./file-download.md)
- [{#T}](../../../../chats/files/index.md)