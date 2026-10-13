# Send Message imbot.v2.Chat.Message.send

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Chat.Message.send` sends a message on behalf of the bot to the specified dialog.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the chat-bot registration ||
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
[`object`](../../../../data-types.md) \| [`array`](../../../../data-types.md) | An attachment: an object with the `BLOCKS` array or a plain array of blocks. More details: [How to use attachments](./attachments/index.md) ||
|| **keyboard**
[`array`](../../../../data-types.md) | Keyboard with buttons. More details: [Working with keyboards](./message-keyboards.md) ||
|| **system**
[`boolean`](../../../../data-types.md) | System message. Allowed values: `true`, `false`. Default is `false` ||
|| **urlPreview**
[`boolean`](../../../../data-types.md) | Show link previews. Allowed values: `true`, `false`. Default is `true` ||
|| **replyId**
[`integer`](../../../../data-types.md) | ID of the message the bot is replying to ||
|| **templateId**
[`string`](../../../../data-types.md) | UUID of the message template ||
|| **forwardIds**
[`object`](../../../../data-types.md) \| [`array`](../../../../data-types.md) | Messages to forward. Two formats are supported:
- an object `{uuid: messageId}`, where `uuid` is an arbitrary UUID string as the key, `messageId` is the ID of the original message. In the response, `uuidMap` will return `{uuid: newMessageId}`, and failed forwards will be listed in `failureMap`. A repeated call with the same `uuid` does not create a new message and returns the ID of the previously created one
- an array `[messageId, ...]`. The IDs of forwarded messages are not returned in the response, and failed forwards will be listed in `legacyFailureMap`

All forwarded messages must be from the same chat. The bot can only forward messages from chats where it is a participant. Maximum of 100 messages.

If some messages fail to forward, the remaining messages are still sent and the method returns a successful response ||
|#

> Boolean parameters accept `true` and `false`. If the client does not support JSON boolean, strings `"Y"` and `"N"` can be passed.

## Code Examples

{% include [Examples Note](../../../../../_includes/examples.md) %}

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

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imbot.v2.chat.message.send(
            bot_id=456,
            dialog_id="chat5",
            fields={
                "message": "Hello from bot!",
                "keyboard": [
                    {
                        "TEXT": "Open",
                        "LINK": "https://example.com",
                        "BG_COLOR_TOKEN": "primary",
                    },
                ],
            },
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "imbot.v2.Chat.Message.send", b24.Params{
    	"botId":    456,
    	"botToken": "my_bot_token",
    	"dialogId": "chat5",
    	"fields": b24.Params{
    		"message": "Hello from bot!",
    		"keyboard": []b24.Params{
    			{
    				"TEXT":           "Open",
    				"LINK":           "https://example.com",
    				"BG_COLOR_TOKEN": "primary",
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("imbot.v2.Chat.Message.send: %w", err)
    }

    var item struct {
    	ID b24.ID `json:"id"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "id": 789,
        "uuidMap": [],
        "failureMap": []
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
[`integer`](../../../../data-types.md) | ID of the sent message.

Returns `null` if the request contains only `forwardIds` without text or attachments ||
|| **result.uuidMap**
[`object`](../../../../data-types.md) | UUID → ID mapping for messages successfully forwarded in the `{uuid: messageId}` format. If there are no such messages, an empty array `[]` is returned ||
|| **result.failureMap**
[`object`](../../../../data-types.md) | UUID → error code mapping for messages that failed to forward in the `{uuid: messageId}` format. The field is always returned. If there are no failed forwards, an empty array `[]` is returned ||
|| **result.legacyFailureMap**
[`object`](../../../../data-types.md) | Original message ID → error code mapping for messages that failed to forward in the `[messageId, ...]` format. The field is returned only if such messages exist ||
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
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token is not specified | `botToken` is not specified. Required for webhook authorization ||
|| `BOT_ID_REQUIRED` | Bot ID is required | `botId` is not specified ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot is registered by another application | Bot is registered by another application ||
|| `ACCESS_DENIED` | Access denied | Bot is not a participant in the chat ||
|| `EMPTY_MESSAGE` | Message is empty | Empty message — no text or attachments ||
|| `SENDING_FAILED` | Sending failed | Error sending message ||
|| `MESSAGES_IN_DIFFERENT_CHAT_ERROR` | MESSAGES_IN_DIFFERENT_CHAT_ERROR | Messages in `forwardIds` are in different chats ||
|| `MESSAGE_NOT_FOUND` | MESSAGE_NOT_FOUND | A message from `forwardIds` was not found or is in a chat where the bot is not a participant ||
|| `WRONG_UUID` | WRONG_UUID | A key in `forwardIds` is not a UUID string ||
|| `TOO_MANY_MESSAGES` | TOO_MANY_MESSAGES | More than 100 messages were passed in `forwardIds` ||
|#

The forwarding errors in the table cancel the entire call: the message is not sent. If no message could be forwarded and no text was passed, the method also returns an error.

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API Change Log for imbot.v2](../../change-log.md)
- [{#T}](./chat-message-update.md)
- [{#T}](./chat-message-delete.md)
- [{#T}](./chat-message-reaction-add.md)
- [{#T}](./attachments/index.md)
- [{#T}](./message-keyboards.md)