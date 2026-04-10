# Responding to the imbot.v2.Command.answer Command

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The `imbot.v2.Command.answer` method sends a bot's response to a slash command invocation.

The bot can respond in the chat from which the command originated, even if the bot is not a participant in that chat. Access is granted temporarily based on the command token — `messageId` + `commandId`. If the bot is not a participant in the chat, the message is sent as a system message indicating the bot's name.

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
|| **commandId***
[`integer`](../../../../data-types.md) | Command ID ||
|| **messageId***
[`integer`](../../../../data-types.md) | Message ID with the command ||
|| **dialogId***
[`string`](../../../../data-types.md) | Dialog ID. For group chats — `chat{chatId}`, for personal chats — `{userId}` ||
|| **fields**
[`object`](../../../../data-types.md) | Fields of the response message. The structure of the object is described [below](#fields) ||
|#

### Fields Parameter {#fields}

#|
|| **Name**
`Type` | **Description** ||
|| **message**
[`string`](../../../../data-types.md) | Response text. Maximum length — 20,000 characters ||
|| **attach**
[`array`](../../../../data-types.md) | Attachments. More details: [How to use attachments](../../../../chats/messages/attachments.md) ||
|| **keyboard**
[`array`](../../../../data-types.md) | Keyboard. More details: [Working with keyboards](../../../../chats/messages/keyboards.md) ||
|| **system**
[`boolean`](../../../../data-types.md) | System message. Allowed values: `true`, `false`. Default is `false` ||
|| **urlPreview**
[`boolean`](../../../../data-types.md) | Show link previews. Allowed values: `true`, `false`. Default is `true` ||
|#

## Code Examples

{% include [Example Note](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","commandId":42,"messageId":789,"dialogId":"chat5","fields":{"message":"Here is the help text..."}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Command.answer
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"commandId":42,"messageId":789,"dialogId":"chat5","fields":{"message":"Here is the help text..."},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Command.answer
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Command.answer', {
        botId: 456,
        commandId: 42,
        messageId: 789,
        dialogId: 'chat5',
        fields: { message: 'Here is the help text...' },
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
                'imbot.v2.Command.answer',
                [
                    'botId' => 456,
                    'commandId' => 42,
                    'messageId' => 789,
                    'dialogId' => 'chat5',
                    'fields' => ['message' => 'Here is the help text...'],
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
        'imbot.v2.Command.answer',
        {
            botId: 456,
            commandId: 42,
            messageId: 789,
            dialogId: 'chat5',
            fields: { message: 'Here is the help text...' },
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
        'imbot.v2.Command.answer',
        [
            'botId' => 456,
            'commandId' => 42,
            'messageId' => 789,
            'dialogId' => 'chat5',
            'fields' => ['message' => 'Here is the help text...'],
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Answer sent';
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
        "date_finish": "2024-10-11T10:00:00+02:00"
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`boolean`](../../../../data-types.md) | `true` if the response was successfully sent ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "COMMAND_ANSWER_FAILED",
    "error_description": "Command answer failed"
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
|| `COMMAND_ANSWER_FAILED` | Command answer failed | Error sending response ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API Change Log for imbot.v2](../../change-log.md)
- [{#T}](./command-register.md)
- [{#T}](../messages/chat-message-send.md)
- [{#T}](../events/event-get.md)