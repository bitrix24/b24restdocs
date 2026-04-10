# Get Message imbot.v2.Chat.Message.get

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Chat.Message.get` returns a message by its ID.

Typical use-case: the bot received an event with `replyId` and wants to read the original message.

{% note warning "" %}

The method is available only for `supervisor` and `personal` type bots. For more details, see [Bot Types](../../../index.md#bot-types).

{% endnote %}

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
|| **messageId***
[`integer`](../../../../data-types.md) | Message ID ||
|#

## Code Examples

{% include [Examples Note](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"botToken":"my_bot_token","messageId":789}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imbot.v2.Chat.Message.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"botId":456,"messageId":789,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imbot.v2.Chat.Message.get
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('imbot.v2.Chat.Message.get', {
        botId: 456,
        messageId: 789,
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
                'imbot.v2.Chat.Message.get',
                [
                    'botId' => 456,
                    'messageId' => 789,
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
        'imbot.v2.Chat.Message.get',
        {
            botId: 456,
            messageId: 789,
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
        'imbot.v2.Chat.Message.get',
        [
            'botId' => 456,
            'messageId' => 789,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: '. $result['error_description'];
    } else {
        echo 'Message: '. $result['result']['message']['text'];
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": {
        "message": {
            "id": 789,
            "chatId": 5,
            "authorId": 1,
            "date": "2026-03-19T14:30:00+02:00",
            "text": "Hello! How are you?",
            "isSystem": false,
            "uuid": "",
            "forward": null,
            "params": {},
            "viewedByOthers": true
        },
        "user": {
            "id": 1,
            "active": true,
            "name": "John Smith",
            "bot": false,
            "type": "employee"
        }
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
|| **result.message**
[`Message`](../../entities.md#message) | Message object [(detailed description)](#message-object) ||
|| **result.user**
[`User`](../../entities.md#user) | Author of the message [(detailed description)](#user-object) ||
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
|| `BOT_OWNERSHIP_ERROR` | Bot is registered by another application | Bot registered by another application ||
|| `BOT_TYPE_NOT_ALLOWED` | Bot type not allowed | Method available only for `supervisor` and `personal` type bots ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API Change Log for imbot.v2](../../change-log.md)
- [{#T}](./chat-message-get-context.md)
- [{#T}](./chat-message-send.md)
- [{#T}](./chat-message-update.md)
- [{#T}](../../index.md#bot-types)