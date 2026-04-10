# Disable Notifications from Chat im.chat.mute

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.chat.mute` disables or enables notifications in the specified chat.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../data-types.md) | The identifier of the chat. You can obtain the identifier using the [im.chat.get](../im-chat-get.md) method ||
|| **DIALOG_ID***
[`string`](../../data-types.md) | The chat identifier in the format:

- `chatXXX` — chat
- `sgXXX` — group or project chat
- `XXX` — user identifier for personal chat

This can be passed instead of `CHAT_ID`. 

The chat identifier can be obtained using the [im.chat.get](../im-chat-get.md) method. The user identifier can be obtained using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods ||
|| **MUTE**
[`string`](../../data-types.md) | Notification control:

- `Y` — disable notifications
- `N` — enable notifications

Default is `Y` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":1489,"MUTE":"Y"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.chat.mute
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":1489,"MUTE":"Y","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/im.chat.mute
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.chat.mute',
            {
                CHAT_ID: 1489,
                MUTE: 'Y'
            }
        );

        const result = response.getData().result;
        console.log('Muted chat:', result);

        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'im.chat.mute',
                [
                    'CHAT_ID' => 1489,
                    'MUTE'    => 'Y'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error muting chat: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.chat.mute',
        {
            CHAT_ID: 1489,
            MUTE: 'Y'
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
        'im.chat.mute',
        [
            'CHAT_ID' => 1489,
            'MUTE'    => 'Y'
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
        "start": 1772541297,
        "finish": 1772541297.419046,
        "duration": 0.41904592514038086,
        "processing": 0,
        "date_start": "2026-03-03T15:34:57+01:00",
        "date_finish": "2026-03-03T15:34:57+01:00",
        "operating_reset_at": 1772541897,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if notifications are successfully disabled or enabled ||
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
- one of the required parameters `CHAT_ID` or `DIALOG_ID` is missing
- an empty or invalid `CHAT_ID` was provided ||
|| `400` | `DIALOG_ID_EMPTY` | Dialog ID can't be empty | An empty or invalid `DIALOG_ID` was provided ||
|| `403` | `ACCESS_ERROR` | This chat cannot be muted | Possible reasons:
- the user does not have access to the chat
- the specified chat does not exist
- the chat does not support disabling notifications ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-recent-pin.md)
- [{#T}](./im-recent-unread.md)
- [{#T}](./im-dialog-read-all.md)
- [{#T}](./im-recent-hide.md)