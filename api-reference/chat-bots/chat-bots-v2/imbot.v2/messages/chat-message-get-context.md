# Get Context of Message imbot.v2.Chat.Message.getContext

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Chat.Message.getContext` returns a window of messages surrounding the specified one. It is used for analyzing the dialogue history.

{% note warning "" %}

The method is only available for `supervisor` and `personal` type bots. For more details, see [Bot Types](../../../index.md#bot-types).

{% endnote %}

## Method Parameters

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the same botToken that was specified during the chat bot registration ||
|| **messageId***
[`integer`](../../../../data-types.md) | ID of the central message ||
|| **range**
[`integer`](../../../../data-types.md) | Number of messages on each side of the central one (1–50). Default is `50` ||
|#

## Code Examples

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","messageId":789,"range":20}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.Message.getContext
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"messageId":789,"range":20,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.Message.getContext
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Chat.Message.getContext', {
        botId: 456,
        messageId: 789,
        range: 20,
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
                'imbot.v2.Chat.Message.getContext',
                [
                    'botId' => 456,
                    'messageId' => 789,
                    'range' => 20,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'result: '. print_r($result, true);
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error: '. $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imbot.v2.Chat.Message.getContext',
        {
            botId: 456,
            messageId: 789,
            range: 20,
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
        'imbot.v2.Chat.Message.getContext',
        [
            'botId' => 456,
            'messageId' => 789,
            'range' => 20,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: '. $result['error_description'];
    } else {
        foreach ($result['result']['messages'] as $message) {
            echo $message['id']. ': '. $message['text']. "\n";
        }
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "messages": [
            {
                "id": 785,
                "chatId": 5,
                "authorId": 1,
                "date": "2026-03-19T14:25:00+02:00",
                "text": "Good afternoon!",
                "isSystem": false,
                "uuid": "",
                "forward": null,
                "params": {},
                "viewedByOthers": true
            },
            {
                "id": 789,
                "chatId": 5,
                "authorId": 2,
                "date": "2026-03-19T14:30:00+02:00",
                "text": "Hi! How are you?",
                "isSystem": false,
                "uuid": "",
                "forward": null,
                "params": {},
                "viewedByOthers": true
            }
        ],
        "users": [
            {
                "id": 1,
                "active": true,
                "name": "John Smith",
                "bot": false,
                "type": "employee"
            },
            {
                "id": 2,
                "active": true,
                "name": "Anna Davis",
                "bot": false,
                "type": "employee"
            }
        ],
        "hasPrevPage": false,
        "hasNextPage": true
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
[`object`](../../../../data-types.md) | Result of the request ||
|| **result.messages**
[`Message[]`](../../entities.md#message) | Array of messages from oldest to newest [(detailed description)](#message-object) ||
|| **result.users**
[`User[]`](../../entities.md#user) | Authors of the messages [(detailed description)](#user-object) ||
|| **result.hasPrevPage**
[`boolean`](../../../../data-types.md) | Are there earlier messages ||
|| **result.hasNextPage**
[`boolean`](../../../../data-types.md) | Are there later messages ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

### Fields of the Message Object {#message-object}

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the message ||
|| **chatId**
[`integer`](../../../../data-types.md) | Identifier of the chat ||
|| **authorId**
[`integer`](../../../../data-types.md) | Identifier of the message author ||
|| **date**
[`string`](../../../../data-types.md) | Date the message was sent ||
|| **text**
[`string`](../../../../data-types.md) | Text of the message ||
|| **isSystem**
[`boolean`](../../../../data-types.md) | System message ||
|| **uuid**
[`string`](../../../../data-types.md) | External identifier of the message ||
|| **forward**
[`object`](../../../../data-types.md) | Data about the forwarded message ||
|| **params**
[`object`](../../../../data-types.md) | Additional parameters of the message ||
|| **viewedByOthers**
[`boolean`](../../../../data-types.md) | Message viewed by other participants ||
|#

### Fields of the User Object {#user-object}

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[`integer`](../../../../data-types.md) | Identifier of the user ||
|| **active**
[`boolean`](../../../../data-types.md) | User is active ||
|| **name**
[`string`](../../../../data-types.md) | User's first and last name ||
|| **bot**
[`boolean`](../../../../data-types.md) | Indicates if the user is a bot ||
|| **type**
[`string`](../../../../data-types.md) | Type of user ||
|#

Complete description of all object fields can be found on the [Objects and Fields](../../entities.md) page.

### Pagination

To load the next page, call the method again, passing the `messageId` of the last message from the current selection. For the previous page, use the ID of the first message.

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "BOT_TYPE_NOT_ALLOWED",
    "error_description": "Bot type not allowed"
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
|| `BOT_TYPE_NOT_ALLOWED` | Bot type not allowed | Method is only available for `supervisor` and `personal` type bots ||
|| `MESSAGE_NOT_FOUND` | Message not found | Message not found ||
|| `MESSAGE_ACCESS_DENIED` | Message access denied | Bot is not a participant in the chat with this message or does not have access to the history ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API Change Log for imbot.v2](../../change-log.md)
- [{#T}](./chat-message-get.md)
- [{#T}](./chat-message-send.md)
- [{#T}](../../index.md#bot-types)