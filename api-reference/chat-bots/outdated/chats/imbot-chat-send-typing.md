# Send Typing Indicator `imbot.chat.sendTyping`

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can execute the method: an authorized user of the application that registered the chat bot. The method only works with bots of this application.

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [imbot.v2.Chat.InputAction.notify](../../chat-bots-v2/imbot.v2/ui/chat-input-action-notify.md).

{% endnote %}

The method `imbot.chat.sendTyping` sends a typing indicator to the dialog. The method returns `true` immediately after the command is sent.

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **BOT_ID**
[`integer`](../../../data-types.md) | The identifier of the chat bot. You can obtain the bot ID using the [imbot.bot.list](../bots/imbot-bot-list.md) method.

If the parameter is not provided, the method looks for the first bot registered by the current application ||
|| **DIALOG_ID*** 
[`string`](../../../data-types.md) | The identifier of the object that will receive the message: user or chat.

Supported formats:
- `USER_ID` — the user identifier, which can be obtained via [user.get](../../../user/user-get.md) or [user.search](../../../user/user-search.md)
- `chatXXX`, where `XXX` is the chat identifier, which can be obtained via [imbot.chat.get](../chats/imbot-chat-get.md) ||
|| **CLIENT_ID**
[`string`](../../../data-types.md) | This parameter is required only for webhooks. Pass the same CLIENT_ID that was specified when registering the chat bot ||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST /
      -H "Content-Type: application/json" /
      -H "Accept: application/json" /
      -d '{"BOT_ID":39,"DIALOG_ID":"chat123","CLIENT_ID":"**put_your_client_id_here**"}' /
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.chat.sendTyping
    ```

- cURL (OAuth)

    ```bash
    curl -X POST /
      -H "Content-Type: application/json" /
      -H "Accept: application/json" /
      -d '{"BOT_ID":39,"DIALOG_ID":"chat123","auth":"**put_access_token_here**"}' /
      https://**put_your_bitrix24_address**/rest/imbot.chat.sendTyping
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.chat.sendTyping', {
        BOT_ID: 39,
        DIALOG_ID: 'chat123',
      });

      const { result } = response.getData();
      console.log('Typing sent:', result);
    } catch (error) {
      console.error('Error sending typing:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imbot.chat.sendTyping',
                [
                    'BOT_ID' => 39,
                    'DIALOG_ID' => 'chat123',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Typing sent: ' . ($result->data() ? 'true' : 'false');
        }
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error sending typing: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.chat.sendTyping',
        {
            BOT_ID: 39,
            DIALOG_ID: 'chat123',
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
        'imbot.chat.sendTyping',
        [
            'BOT_ID' => 39,
            'DIALOG_ID' => 'chat123',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Typing sent: ' . ($result['result'] ? 'true' : 'false');
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": true,
    "time": {
        "start": 1728626400.123,
        "finish": 1728626400.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2024-10-11T10:00:00+02:00",
        "date_finish": "2024-10-11T10:00:00+02:00",
        "operating_reset_at": 1762349466,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | `true` if the typing indicator command was accepted ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "DIALOG_ID_EMPTY",
    "error_description": "Dialog ID can't be empty"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_ID_ERROR` | Bot not found | The bot is not found or the application does not have an available bot for auto-completion of `BOT_ID` ||
|| `APP_ID_ERROR` | Bot was installed by another REST application | The provided `BOT_ID` belongs to another application ||
|| `DIALOG_ID_EMPTY` | Dialog ID can't be empty | The dialog identifier was not provided ||
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
- [Chat Bot Exit from Specified Chat imbot.chat.leave](./imbot-chat-leave.md)
