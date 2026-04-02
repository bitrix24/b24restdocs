# Get a Link to Download a File im.v2.File.download

> Scope: [`im`](../../../../scopes/permissions.md)
>
> Who can execute the method: authorized user

The method `im.v2.File.download` returns a link to download a file from a chat.

## Method Parameters

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **dialogId***
[`string`](../../../../data-types.md) | ID of the dialog. For group chats — `chat{chatId}`, for personal chats — `{userId}` ||
|| **fileId***
[`integer`](../../../../data-types.md) | ID of the file on Drive. Can be obtained from the response of the [im.v2.File.upload](./file-upload.md) method ||
|#

## Code Examples

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"dialogId":"chat5","fileId":138}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.v2.File.download
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"dialogId":"chat5","fileId":138,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.v2.File.download
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.v2.File.download', {
        dialogId: 'chat5',
        fileId: 138,
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
                'im.v2.File.download',
                [
                    'dialogId' => 'chat5',
                    'fileId' => 138,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'result: ' . print_r($result, true);
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.v2.File.download',
        {
            dialogId: 'chat5',
            fileId: 138,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                const downloadUrl = result.data().downloadUrl;
                window.open(downloadUrl);
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.v2.File.download',
        [
            'dialogId' => 'chat5',
            'fileId' => 138,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Download URL: ' . $result['result']['downloadUrl'];
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "downloadUrl": "https://**put_your_bitrix24_address**/rest/download.json?token=im%7CaW..."
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
|| **result.downloadUrl**
[`string`](../../../../data-types.md) | A one-time link to download the file. The link contains an authorization token, reuse is not guaranteed ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "FILE_NOT_FOUND",
    "error_description": "File not found"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `FILE_NOT_FOUND` | File not found | The file was not found in the specified chat ||
|| `FILE_ACCESS_ERROR` | File access error | No permission to download the file — the file does not belong to the specified chat ||
|| `ACCESS_DENIED` | Access denied | No access to the chat ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [Upload a File to Chat im.v2.File.upload](./file-upload.md)