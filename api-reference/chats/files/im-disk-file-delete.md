# Delete File from Chat Folder im.disk.file.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: a chat participant who sent the file

The method `im.disk.file.delete` removes a file from the chat folder. Only the user—who is a participant in the chat and sent the file—can delete it. Other chat participants cannot delete the file.

After deletion, the text *Message deleted* is displayed in the chat instead of the file.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../data-types.md) | Identifier of the chat ||
|| **FILE_ID***
[`integer`](../../data-types.md) | Identifier of the file ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CHAT_ID":1489,"FILE_ID":5163}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.disk.file.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CHAT_ID":1489,"FILE_ID":5163,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.disk.file.delete
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.disk.file.delete',
            {
                CHAT_ID: 1489,
                FILE_ID: 5163
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
                'im.disk.file.delete',
                [
                    'CHAT_ID' => 1489,
                    'FILE_ID' => 5163,
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
        'im.disk.file.delete',
        {
            CHAT_ID: 1489,
            FILE_ID: 5163
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
        'im.disk.file.delete',
        [
            'CHAT_ID' => 1489,
            'FILE_ID' => 5163,
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
    "result": true,
    "time": {
        "start": 1772194631,
        "finish": 1772194631.786718,
        "duration": 0.7867178916931152,
        "processing": 0,
        "date_start": "2026-02-27T15:17:11+01:00",
        "date_finish": "2026-02-27T15:17:11+01:00",
        "operating_reset_at": 1772195231,
        "operating": 0.2657620906829834
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true`:
- if the file is successfully deleted,
- the user is a participant in the chat but did not send the file. In this case, the file will not be deleted.

The method returns `false`:
- if the user does not have access to the chat,
- if an incorrect `CHAT_ID` or `FILE_ID` is specified ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
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
|| `400` | `CHAT_ID_EMPTY` | Chat ID can't be empty | The required parameter `CHAT_ID` is missing or empty ||
|| `400` | `FILE_ID_EMPTY` | File ID can't be empty | The required parameter `FILE_ID` is missing or empty ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-disk-file-commit.md)
- [{#T}](./im-disk-file-save.md)
- [{#T}](./im-disk-folder-get.md)