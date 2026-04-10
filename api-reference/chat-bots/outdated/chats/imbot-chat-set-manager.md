# Assign or Revoke Chat Administrator Rights imbot.chat.setManager

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: chat owner

{% note warning "DEPRECATED" %}

Development of this method has been halted. To assign or revoke administrator rights, use the methods [imbot.v2.Chat.Manager.add](../../chat-bots-v2/imbot.v2/chats/chat-manager-add.md) and [imbot.v2.Chat.Manager.delete](../../chat-bots-v2/imbot.v2/chats/chat-manager-delete.md).

{% endnote %}

The method `imbot.chat.setManager` assigns a chat administrator or revokes administrator rights from a chat participant.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../../data-types.md) | The identifier of the chat.

The identifier can be obtained using the method [imbot.chat.get](./imbot-chat-get.md) ||
|| **USER_ID***
[`integer`](../../../data-types.md) | The identifier of the user for whom the administrator status is being changed.

The user identifier can be obtained using the method [imbot.chat.user.list](./imbot-chat-user-list.md) ||
|| **IS_MANAGER**
[`string`](../../../data-types.md) | Administrator status. Possible values:
- `Y` — assign as administrator
- `N` — revoke administrator rights

Default is `Y` ||
|| **BOT_ID**
[`integer`](../../../data-types.md) | The identifier of the chat bot. The bot identifier can be obtained using the method [imbot.bot.list](../bots/imbot-bot-list.md).

If this parameter is not provided, the method searches for the first bot registered by the current application ||
|| **CLIENT_ID**
[`string`](../../../data-types.md) | This parameter is required only for webhooks. Pass the same CLIENT_ID that was specified during the chat bot registration ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST /
    -H "Content-Type: application/json" /
    -H "Accept: application/json" /
    -d '{"CHAT_ID":2725,"USER_ID":1269,"IS_MANAGER":"Y","CLIENT_ID":"**put_your_client_id_here**"}' /
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.chat.setManager
    ```

- cURL (OAuth)

    ```bash
    curl -X POST /
    -H "Content-Type: application/json" /
    -H "Accept: application/json" /
    -d '{"CHAT_ID":2725,"USER_ID":1269,"IS_MANAGER":"Y","auth":"**put_access_token_here**"}' /
    https://**put_your_bitrix24_address**/rest/imbot.chat.setManager
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imbot.chat.setManager',
            {
                CHAT_ID: 2725,
                USER_ID: 1269,
                IS_MANAGER: 'Y'
            }
        );
        
        const result = response.getData().result;
        console.log(result);
        
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
                'imbot.chat.setManager',
                [
                    'CHAT_ID' => 2725,
                    'USER_ID' => 1269,
                    'IS_MANAGER' => 'Y'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting chat manager: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.chat.setManager',
        {
            CHAT_ID: 2725,
            USER_ID: 1269,
            IS_MANAGER: 'Y'
        },
        function (result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imbot.chat.setManager',
        [
            'CHAT_ID' => 2725,
            'USER_ID' => 1269,
            'IS_MANAGER' => 'Y'
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
        "start": 1772543706,
        "finish": 1772543706.871191,
        "duration": 0.8711910247802734,
        "processing": 0,
        "date_start": "2026-03-03T16:15:06+01:00",
        "date_finish": "2026-03-03T16:15:06+01:00",
        "operating_reset_at": 1772544306,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | `true` if the administrator rights were changed ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "WRONG_REQUEST",
    "error_description": "Change manager can only owner and user must be member in chat"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided ||
|| `USER_ID_EMPTY` | User ID can't be empty | `USER_ID` not provided ||
|| `WRONG_REQUEST` | Change manager can only owner and user must be member in chat | Only the chat owner can change administrator rights, and the user must be a member of the chat ||
|| `BOT_ID_ERROR` | Bot not found | Chat bot not found ||
|| `APP_ID_ERROR` | Bot was installed by another rest application | Chat bot installed by another application ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [Create a chat on behalf of the chat bot imbot.chat.add](./imbot-chat-add.md)
- [Add Participants to Chat imbot.chat.user.add](./imbot-chat-user-add.md)
- [Change Chat Title imbot.chat.updateTitle](./imbot-chat-update-title.md)
- [Change Chat Avatar imbot.chat.updateAvatar](./imbot-chat-update-avatar.md)
- [Change Chat Color imbot.chat.updateColor](./imbot-chat-update-color.md)
- [Get Chat ID imbot.chat.get](./imbot-chat-get.md)
- [Get Data About Chat imbot.dialog.get](./imbot-dialog-get.md)
- [Get the List of Chat Participants imbot.chat.user.list](./imbot-chat-user-list.md)
- [Exclude Participants from Chat imbot.chat.user.delete](./imbot-chat-user-delete.md)
- [Chat Bot Exit from Specified Chat imbot.chat.leave](./imbot-chat-leave.md)
