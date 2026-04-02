# Upload a File to Chat im.v2.File.upload

> Scope: [`im`](../../../../scopes/permissions.md)
>
> Who can execute the method: authorized user

The method `im.v2.File.upload` uploads a file to the chat on behalf of the current user. It combines three steps of the deprecated API into a single call: uploading the file to the Drive, attaching it to the chat, and sending a message.

## Method Parameters

{% include [Note on parameters](../../../../../_includes/required.md) %}

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
[`string`](../../../../data-types.md) | File content in [Base64](../../../../files/how-to-upload-files.md) encoding. Maximum size — 100 MB ||
|| **message**
[`string`](../../../../data-types.md) | Text of the message sent along with the file ||
|#

{% note info "" %}

How to prepare the value for `fields.content`:

1. Read the file in binary mode.
2. Encode the content in Base64.
3. Pass only the Base64 string, without the prefix `data:*/*;base64,`.

More details: [How to upload files](../../../../files/how-to-upload-files.md#how-to-encode-file-to-base64).

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"dialogId":"chat5","fields":{"name":"report.pdf","content":"SGVsbG8gV29ybGQh","message":"Here is the report"}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.v2.File.upload
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"dialogId":"chat5","fields":{"name":"report.pdf","content":"SGVsbG8gV29ybGQh","message":"Here is the report"},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.v2.File.upload
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.v2.File.upload', {
        dialogId: 'chat5',
        fields: {
          name: 'report.pdf',
          content: 'SGVsbG8gV29ybGQh',
          message: 'Here is the report',
        },
      });

      const { result } = response.getData();
      console.log('result:', result);
    } catch (error) {
      console.error('Error:', error);
    }
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
                        'name' => 'report.pdf',
                        'content' => base64_encode(file_get_contents('/path/to/report.pdf')),
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
                name: 'report.pdf',
                content: btoa('...'),
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
                'name' => 'report.pdf',
                'content' => base64_encode(file_get_contents('/path/to/report.pdf')),
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

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "dialogId": "chat5",
        "chatId": 5,
        "messageId": 123,
        "file": {
            "id": 138,
            "chatId": 5,
            "name": "report.pdf",
            "extension": "pdf",
            "size": 35341
        }
    },
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+02:00",
        "date_finish": "2024-10-11T10:00:00+02:00"
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | Result of the operation ||
|| **result.dialogId**
[`string`](../../../../data-types.md) | ID of the dialog ||
|| **result.chatId**
[`integer`](../../../../data-types.md) | Numeric ID of the chat ||
|| **result.messageId**
[`integer`](../../../../data-types.md) | ID of the created message with the file ||
|| **result.file**
[`File`](../../entities.md#file) | Data of the uploaded file [(detailed description)](#file-object) ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

### Fields of the File Object {#file-object}

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the file ||
|| **chatId**
[`integer`](../../../../data-types.md) | Identifier of the chat ||
|| **name**
[`string`](../../../../data-types.md) | Name of the file ||
|| **extension**
[`string`](../../../../data-types.md) | File extension ||
|| **size**
[`integer`](../../../../data-types.md) | Size of the file in bytes ||
|#

A complete description of all object fields can be found on the [Objects and Fields](../../entities.md) page.

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "FILE_TOO_LARGE",
    "error_description": "File too large"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `FILE_EMPTY` | File name or content is empty | File name or content is not specified ||
|| `INVALID_CONTENT` | Invalid base64 content | Invalid Base64 ||
|| `FOLDER_ERROR` | Failed to get chat folder | Failed to retrieve chat folder ||
|| `UPLOAD_FAILED` | File upload failed | File upload error ||
|| `SEND_FAILED` | Failed to send message | Message sending error ||
|| `FILE_TOO_LARGE` | File is too large | File size exceeds 100 MB ||
|| `ACCESS_DENIED` | Access denied | No access to the chat ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [Get a Link to Download a File im.v2.File.download](./file-download.md)