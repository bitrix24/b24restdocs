# Save File to Your Drive im.disk.file.save

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: chat participant

The method `im.disk.file.save` saves a file from the chat to the user's personal Drive.

The file is saved in the *Saved Files* folder. If the folder does not exist, the system will create it automatically.

## Method Parameters

{% include [Footnote on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FILE_ID***
[`integer`](../../data-types.md) | File identifier ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"FILE_ID":5155}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.disk.file.save
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"FILE_ID":5155,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.disk.file.save
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.disk.file.save',
            {
                FILE_ID: 5155
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
                'im.disk.file.save',
                [
                    'FILE_ID' => 5155,
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
        'im.disk.file.save',
        {
            FILE_ID: 5155
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
        'im.disk.file.save',
        [
            'FILE_ID' => 5155,
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
        "folder": {
            "id": 4821,
            "name": "Saved Files"
        },
        "file": {
            "id": 5159,
            "name": "image.jpg"
        }
    },
    "time": {
        "start": 1772193101,
        "finish": 1772193101.625023,
        "duration": 0.6250228881835938,
        "processing": 0,
        "date_start": "2026-02-27T14:51:41+01:00",
        "date_finish": "2026-02-27T14:51:41+01:00",
        "operating_reset_at": 1772193701,
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
|| **result.folder**
[`object`](../../data-types.md) | The *Saved Files* folder on the personal Drive [(detailed description)](#folder) ||
|| **result.file**
[`object`](../../data-types.md) | The saved file [(detailed description)](#file) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### Folder Object {#folder}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Folder identifier ||
|| **name**
[`string`](../../data-types.md) | Folder name — *Saved Files* ||
|#

### File Object {#file}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the saved file ||
|| **name**
[`string`](../../data-types.md) | Name of the saved file ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "FILE_ID_EMPTY",
    "error_description": "File ID can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `FILE_ID_EMPTY` | File ID can't be empty | The required parameter `FILE_ID` is missing or empty ||
|| `400` | `FILE_SAVE_ERROR` | File ID can't be saved | Possible reasons:
- The user does not have access to the chat from which the file needs to be saved
- The specified file was not found
- Drive is disabled
- Failed to save the file in the *Saved Files* folder, for example, due to lack of space
- Failed to create the *Saved Files* folder ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-disk-file-commit.md)
- [{#T}](./im-disk-file-delete.md)
- [{#T}](./im-disk-folder-get.md)