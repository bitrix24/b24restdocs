# Change Chat Avatar imbot.chat.updateAvatar

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: authorized user of the application that registered the chat bot

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [imbot.v2.Chat.update](../../chat-bots-v2/imbot.v2/chats/chat-update.md).

{% endnote %}

The method `imbot.chat.updateAvatar` updates the chat avatar.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../../data-types.md) | Identifier of the chat.

The identifier can be obtained using the method [imbot.chat.get](./imbot-chat-get.md) ||
|| **AVATAR***
[`string`](../../../data-types.md) | Image in [Base64](../../../files/how-to-upload-files.md) format.

Maximum image size is 5000x5000 ||
|| **BOT_ID**
[`integer`](../../../data-types.md) | Identifier of the chat bot. You can obtain the bot identifier using the method [imbot.bot.list](../bots/imbot-bot-list.md).

If this parameter is not provided, the method searches for the first bot registered by the current application ||
|| **CLIENT_ID**
[`string`](../../../data-types.md) | This parameter is required only for webhooks. Pass the same CLIENT_ID that was specified during the registration of the chat bot ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST /
    -H "Content-Type: application/json" /
    -H "Accept: application/json" /
    -d '{"CHAT_ID":2725,"AVATAR":"/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k=","CLIENT_ID":"**put_your_client_id_here**"}' /
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.chat.updateAvatar
    ```

- cURL (OAuth)

    ```bash
    curl -X POST /
    -H "Content-Type: application/json" /
    -H "Accept: application/json" /
    -d '{"CHAT_ID":2725,"AVATAR":"/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k=","auth":"**put_access_token_here**"}' /
    https://**put_your_bitrix24_address**/rest/imbot.chat.updateAvatar
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imbot.chat.updateAvatar',
            {
                CHAT_ID: 2725,
                AVATAR: '/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k='
            }
        );
        
        const result = response.getData().result;
        console.log('Updated chat avatar:', result);
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
                'imbot.chat.updateAvatar',
                [
                    'CHAT_ID' => 2725,
                    'AVATAR' => '/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k='
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating chat avatar: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.chat.updateAvatar',
        {
            CHAT_ID: 2725,
            AVATAR: '/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k=',
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
        'imbot.chat.updateAvatar',
        [
            'CHAT_ID' => 2725,
            'AVATAR' => '/9j/4QAYRXhpZgAAS...CgCgCgCgP/9k='
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
        "start": 1771934650,
        "finish": 1771934651.123539,
        "duration": 1.1235389709472656,
        "processing": 1,
        "date_start": "2026-02-24T15:04:10+01:00",
        "date_finish": "2026-02-24T15:04:11+01:00",
        "operating_reset_at": 1771935250,
        "operating": 0.9618949890136719
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | `true` if the avatar was updated ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "AVATAR_ERROR",
    "error_description": "Avatar incorrect type"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided ||
|| `ACCESS_DENIED` | Access denied | Insufficient permissions to change the avatar ||
|| `ACCESS_ERROR` | Action unavailable | Operation not available for this chat ||
|| `ACCESS_ERROR` | The avatar of this chat cannot be changed | Cannot change the avatar of this chat ||
|| `AVATAR_ERROR` | Avatar incorrect type | The file provided is not of image type ||
|| `AVATAR_ERROR` | Avatar incorrect size (max 5000x5000) | The image size exceeds the limit ||
|| `WRONG_REQUEST` | Chat doesn't exist | The specified chat does not exist ||
|| `BOT_ID_ERROR` | Bot not found | Chat bot not found ||
|| `APP_ID_ERROR` | Bot was installed by another REST application | Chat bot installed by another application ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [Create a chat on behalf of the chat bot imbot.chat.add](./imbot-chat-add.md)
- [Add Participants to Chat imbot.chat.user.add](./imbot-chat-user-add.md)
- [Assign or Revoke Chat Administrator Rights imbot.chat.setManager](./imbot-chat-set-manager.md)
- [Change Chat Title imbot.chat.updateTitle](./imbot-chat-update-title.md)
- [Change Chat Color imbot.chat.updateColor](./imbot-chat-update-color.md)
- [Get Chat ID imbot.chat.get](./imbot-chat-get.md)
- [Get Data About Chat imbot.dialog.get](./imbot-dialog-get.md)
- [Get the List of Chat Participants imbot.chat.user.list](./imbot-chat-user-list.md)
- [Exclude Participants from Chat imbot.chat.user.delete](./imbot-chat-user-delete.md)
- [Chat Bot Exit from Specified Chat imbot.chat.leave](./imbot-chat-leave.md)
