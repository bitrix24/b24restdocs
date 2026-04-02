# Send Message imbot.v2.Chat.Message.send

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Chat.Message.send` sends a message on behalf of the bot to the specified dialog.

## Method Parameters

{% include [Parameter Note](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the chat bot registration ||
|| **dialogId***
[`string`](../../../../data-types.md) | Dialog ID. For group chats — `chat{chatId}`, for personal chats — `{userId}` ||
|| **fields**
[`object`](../../../../data-types.md) | Message content. The structure of the object is described [below](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`Type` | **Description** ||
|| **message**
[`string`](../../../../data-types.md) | Message text. Maximum length — 20,000 characters. If exceeded, the text is truncated with ` (...)` added ||
|| **attach**
[`array`](../../../../data-types.md) | Attachments. More details: [How to use attachments](./attachments/index.md) ||
|| **keyboard**
[`array`](../../../../data-types.md) | Keyboard with buttons. More details: [Working with Keyboards](./message-keyboards.md) ||
|| **system**
[`boolean`](../../../../data-types.md) | System message. Allowed values: `true`, `false`. Default is `false` ||
|| **urlPreview**
[`boolean`](../../../../data-types.md) | Show link previews. Allowed values: `true`, `false`. Default is `true` ||
|| **replyId**
[`integer`](../../../../data-types.md) | ID of the message the bot is replying to ||
|| **templateId**
[`string`](../../../../data-types.md) | UUID of the message template ||
|| **forwardIds**
[`object`](../../../../data-types.md) | Messages to forward. Format: `{uuid: messageId}`, where `uuid` is an arbitrary UUID string as the key, `messageId` is the ID of the original message. In the response, `uuidMap` will return `{uuid: newMessageId}`.

The bot can only forward messages from chats where it is a participant. Maximum of 100 messages ||
|#

## Code Examples

{% include [Example Note](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","dialogId":"chat5","fields":{"message":"Hello from bot!","keyboard":[{"TEXT":"Open","LINK":"https://example.com","BG_COLOR_TOKEN":"primary"}]}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.Message.send
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"dialogId":"chat5","fields":{"message":"Hello from bot!"},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.Message.send
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Chat.Message.send', {
        botId: 456,
        dialogId: 'chat5',
        fields: {
          message: 'Hello from bot!',
          keyboard: [
            { TEXT: 'Open', LINK: 'https://example.com', BG_COLOR_TOKEN: 'primary' },
          ],
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
                'imbot.v2.Chat.Message.send',
                [
                    'botId' => 456,
                    'dialogId' => 'chat5',
                    'fields' => [
                        'message' => 'Hello from bot!',
                        'keyboard' => [
                            ['TEXT' => 'Open', 'LINK' => 'https://example.com', 'BG_COLOR_TOKEN' => 'primary'],
                        ],
                    ],
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
        'imbot.v2.Chat.Message.send',
        {
            botId: 456,
            dialogId: 'chat5',
            fields: {
                message: 'Hello from bot!',
                keyboard: [
                    { TEXT: 'Open', LINK: 'https://example.com', BG_COLOR_TOKEN: 'primary' },
                ],
            },
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log('Message ID:', result.data().id);
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imbot.v2.Chat.Message.send',
        [
            'botId' => 456,
            'dialogId' => 'chat5',
            'fields' => [
                'message' => 'Hello from bot!',
            ],
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Message ID: ' . $result['result']['id'];
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "id": 789,
        "uuidMap": {}
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
[`object`](../../../../data-types.md) | Result of the message sending ||
|| **result.id**
[`integer`](../../../../data-types.md) | ID of the sent message ||
|| **result.uuidMap**
[`object`](../../../../data-types.md) | UUID → ID mapping for forwarded messages ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "EMPTY_MESSAGE",
    "error_description": "Message is empty"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token is not specified | `botToken` is not provided. Required for webhook authorization ||
|| `BOT_ID_REQUIRED` | Bot ID is required | `botId` is not provided ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot is registered by another application | Bot is registered by another application ||
|| `ACCESS_DENIED` | Access denied | Bot is not a participant in the chat ||
|| `EMPTY_MESSAGE` | Message is empty | Empty message — no text or attachments ||
|| `SENDING_FAILED` | Sending failed | Error sending the message ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [Update Message imbot.v2.Chat.Message.update](./chat-message-update.md)
- [Delete Message imbot.v2.Chat.Message.delete](./chat-message-delete.md)
- [Add Reaction to Message imbot.v2.Chat.Message.Reaction.add](./chat-message-reaction-add.md)
- [Attachments in Messages ATTACH](./attachments/index.md)
- [Working with Keyboards](./message-keyboards.md)