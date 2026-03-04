# Get Chat File Storage Folder im.disk.folder.get

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat participant

The method `im.disk.folder.get` retrieves the identifier of the folder where chat files are stored.

The identifier from the response can be used in Drive methods:
- [disk.folder.uploadfile](../../disk/folder/disk-folder-upload-file.md) to upload a file to the chat folder
- [disk.folder.getchildren](../../disk/folder/disk-folder-get-children.md) to get a list of files in the folder

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../data-types.md) | Identifier of the chat. Required if `DIALOG_ID` is not provided ||
|| **DIALOG_ID***
[`string`](../../data-types.md) | Identifier of the dialog in the format `chatXXX`, where `XXX` is the identifier. Required if `CHAT_ID` is not provided ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat1489"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.disk.folder.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"DIALOG_ID":"chat1489","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.disk.folder.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.disk.folder.get',
            {
                DIALOG_ID: 'chat1489'
            }
        );

        console.log(response.getData().result);
    }
    catch (error)
    {
        console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.disk.folder.get',
                [
                    'DIALOG_ID' => 'chat1489',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.disk.folder.get',
        {
            DIALOG_ID: 'chat1489'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'im.disk.folder.get',
        [
            'DIALOG_ID' => 'chat1489',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "ID": 5153
    },
    "time": {
        "start": 1772192121,
        "finish": 1772192121.447936,
        "duration": 0.4479360580444336,
        "processing": 0,
        "date_start": "2026-02-27T14:35:21+02:00",
        "date_finish": "2026-02-27T14:35:21+02:00",
        "operating_reset_at": 1772192721,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **result.ID**
[`integer`](../../data-types.md) | Identifier of the chat file storage folder ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat ID can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CHAT_ID_EMPTY` | Chat ID can't be empty | Possible reasons:
- one of the required parameters is missing: `CHAT_ID` or `DIALOG_ID`
- an empty `CHAT_ID` was provided ||
|| `400` | `DIALOG_ID_EMPTY` | Dialog ID can't be empty | An invalid or empty `DIALOG_ID` was provided ||
|| `403` | `ACCESS_ERROR` | You do not have access to the specified dialog | Insufficient permissions to view the dialog or a non-existent dialog was provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-disk-file-commit.md)
- [{#T}](./im-disk-file-save.md)
- [{#T}](./im-disk-file-delete.md)