# Chat Bot Exit from Specified Chat imbot.chat.leave

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: a user of the application that registered the chat bot

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [imbot.v2.Chat.leave](../../chat-bots-v2/imbot.v2/chats/chat-leave.md).

{% endnote %}

The method `imbot.chat.leave` removes the chat bot from the chat.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../../data-types.md) | Identifier of the chat.

The identifier can be obtained using the method [imbot.chat.get](./imbot-chat-get.md) ||
|| **BOT_ID**
[`integer`](../../../data-types.md) | Identifier of the chat bot. You can get the bot's identifier using the method [imbot.bot.list](../bots/imbot-bot-list.md).

If this parameter is not provided, the method searches for the first bot registered by the current application ||
|| **CLIENT_ID**
[`string`](../../../data-types.md) | This parameter is required only for webhooks. Pass the same CLIENT_ID that was specified during the registration of the chat bot ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST /
    -H "Content-Type: application/json" /
    -H "Accept: application/json" /
    -d '{"CHAT_ID":13,"BOT_ID":39,"CLIENT_ID":"**put_your_client_id_here**"}' /
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.chat.leave
    ```

- cURL (OAuth)

    ```bash
    curl -X POST /
    -H "Content-Type: application/json" /
    -H "Accept: application/json" /
    -d '{"CHAT_ID":13,"BOT_ID":39,"auth":"**put_access_token_here**"}' /
    https://**put_your_bitrix24_address**/rest/imbot.chat.leave
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.chat.leave', {
        CHAT_ID: 13,
        BOT_ID: 39
      });

      console.log('Result:', response.getData().result);
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
                'imbot.chat.leave',
                [
                    'CHAT_ID' => 2729,
                    'BOT_ID' => 1291
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error leaving chat: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.chat.leave',
        {
            CHAT_ID: 2729,
            BOT_ID: 1291
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
        'imbot.chat.leave',
        [
            'CHAT_ID' => 13,
            'BOT_ID' => 39,
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
        "start": 1771946333,
        "finish": 1771946334.073154,
        "duration": 1.0731539726257324,
        "processing": 1,
        "date_start": "2026-02-24T16:18:53+03:00",
        "date_finish": "2026-02-24T16:18:54+03:00",
        "operating_reset_at": 1771946933,
        "operating": 0.10132789611816406
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | `true` if the chat bot has exited the chat ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "CHAT_ID_EMPTY",
    "error_description": "Chat ID can't be empty"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `CHAT_ID_EMPTY` | Chat ID can't be empty | `CHAT_ID` not provided ||
|| `ACCESS_ERROR` | Action unavailable | Operation not available for this chat ||
|| `ACCESS_ERROR` | It is forbidden to delete users of this chat | Cannot remove the chat bot from this chat ||
|| `ACCESS_ERROR` | LEAVE_OWNER_FORBIDDEN | Cannot remove the chat owner without transferring rights ||
|| `WRONG_REQUEST` | You don't have access or user isn't a member in chat | No rights to leave or the chat bot is not a participant in the chat ||
|| `BOT_ID_ERROR` | Bot not found | Chat bot not found ||
|| `APP_ID_ERROR` | Bot was installed by another rest application | Chat bot installed by another application ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [Create a chat on behalf of the chat bot imbot.chat.add](./imbot-chat-add.md)
- [Add Participants to Chat imbot.chat.user.add](./imbot-chat-user-add.md)
- [Assign or Revoke Chat Administrator Rights imbot.chat.setManager](./imbot-chat-set-manager.md)
- [Change Chat Title imbot.chat.updateTitle](./imbot-chat-update-title.md)
- [Change Chat Avatar imbot.chat.updateAvatar](./imbot-chat-update-avatar.md)
- [Change Chat Color imbot.chat.updateColor](./imbot-chat-update-color.md)
- [Get Chat ID imbot.chat.get](./imbot-chat-get.md)
- [Get Data About Chat imbot.dialog.get](./imbot-dialog-get.md)
- [Get the List of Chat Participants imbot.chat.user.list](./imbot-chat-user-list.md)
- [Exclude Participants from Chat imbot.chat.user.delete](./imbot-chat-user-delete.md)
