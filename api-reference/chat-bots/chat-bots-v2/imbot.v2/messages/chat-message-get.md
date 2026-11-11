# Get Message imbot.v2.Chat.Message.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can execute the method: owner of the registered bot

The method `imbot.v2.Chat.Message.get` returns a message by its ID.

For example, the bot received an event with a reply to a message and wants to read the original one. The ID of the original message comes in `message.params.REPLY_ID` — pass it in `messageId`.

{% note warning "" %}

The method is available only for `supervisor` and `personal` type bots. For more details, see [Bot Types](../../index.md#bot-types).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **botId***
[`integer`](../../../../data-types.md) | Bot ID, an integer greater than 0 ||
|| **botToken**
[`string`](../../../../data-types.md) | Unique authorization token for the bot. Required for webhook authorization, not needed for OAuth.

Pass the current token: the one specified when registering the bot, or a new one if the token was changed with the [imbot.v2.Bot.update](../bots/bot-update.md) method. With an old token, the method returns the `BOT_OWNERSHIP_ERROR` error ||
|| **messageId***
[`integer`](../../../../data-types.md) | Message ID. You can take it from `message.id` of the [ONIMBOTV2MESSAGEADD](../events/events.md#onimbotv2messageadd) event ||
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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type MessageGetResult = {
      message: {
        id: number
        chatId: number
        authorId: number
        text: string
      }
      user: {
        id: number | null
        name: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<MessageGetResult>({
        method: 'imbot.v2.Chat.Message.get',
        params: {
          botId: 456,
          botToken: 'my_bot_token',
          messageId: 789,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Message:', result.message.id, '| Text:', result.message.text)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getMessage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'imbot.v2.Chat.Message.get',
            params: {
              botId: 456,
              botToken: 'my_bot_token',
              messageId: 789,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Message:', result.message.id, '| Text:', result.message.text)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getMessage)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imbot.v2.chat.message.get(
            bot_id=456,
            bot_token="my_bot_token",
            message_id=789,
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
                'imbot.v2.Chat.Message.get',
                [
                    'botId' => 456,
                    'botToken' => 'my_bot_token',
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
            botToken: 'my_bot_token',
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
            'botToken' => 'my_bot_token',
            'messageId' => 789,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: '. $result['error_description'];
    } else {
        echo 'Message: '. $result['result']['message']['text'];
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "imbot.v2.Chat.Message.get", b24.Params{
    	"botId":     456,
    	"botToken":  "my_bot_token",
    	"messageId": 789,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("imbot.v2.Chat.Message.get: %w", err)
    }

    keys, ok := b24.Keys(res.Result)
    if !ok {
    	return fmt.Errorf("expected an object in the response")
    }
    fmt.Println("fields in response:", len(keys))
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

The example does not show all fields of the message and the user. For the full set, see the descriptions of the [Message](../../entities.md#message) and [User](../../entities.md#user) objects.

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
            "uuid": null,
            "forward": null,
            "params": [],
            "viewedByOthers": true
        },
        "user": {
            "id": 1,
            "active": true,
            "name": "John Smith",
            "bot": false,
            "type": "user"
        }
    },
    "time": {
        "start": 1773919860.123,
        "finish": 1773919860.234,
        "duration": 0.111,
        "processing": 0.045,
        "date_start": "2026-03-19T14:31:00+02:00",
        "date_finish": "2026-03-19T14:31:00+02:00"
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
[`integer`](../../../../data-types.md) | Message identifier ||
|| **chatId**
[`integer`](../../../../data-types.md) | Chat identifier ||
|| **authorId**
[`integer`](../../../../data-types.md) | Identifier of the message author. `0` for system messages ||
|| **date**
[`string`](../../../../data-types.md) | Date of message sending ||
|| **text**
[`string`](../../../../data-types.md) | Text of the message ||
|| **isSystem**
[`boolean`](../../../../data-types.md) | System message ||
|| **uuid**
[```string|null```](../../../../data-types.md) | Message UUID if it was passed when sending, or `null` ||
|| **forward**
[```object|null```](../../../../data-types.md) | Forwarding data if the message was forwarded, or `null`. For the field descriptions, see the [Message](../../entities.md#message) object ||
|| **params**
[```object|array```](../../../../data-types.md) | Additional message parameters: attachments, keyboard, files, and others. If there are no parameters — an empty array `[]`. Key descriptions — [params Object](../../entities.md#message-params) ||
|| **viewedByOthers**
[`boolean`](../../../../data-types.md) | Message viewed by other participants ||
|| **chat_id**
[`integer`](../../../../data-types.md) | Same as `chatId`. A legacy-format field ||
|| **author_id**
[`integer`](../../../../data-types.md) | Same as `authorId`. A legacy-format field ||
|| **block**
[```object|null```](../../../../data-types.md) | Internal field, `null` for regular messages ||
|#

### Fields of the User Object {#user-object}

#|
|| **Field**
`Type` | **Description** ||
|| **id**
[```integer|null```](../../../../data-types.md) | User identifier. `null` for system messages ||
|| **active**
[`boolean`](../../../../data-types.md) | User is active ||
|| **name**
[`string`](../../../../data-types.md) | User's first and last name ||
|| **bot**
[`boolean`](../../../../data-types.md) | Indicates if the user is a bot ||
|| **type**
[`string`](../../../../data-types.md) | User type, for example `user` — an employee, `bot` — a chatbot ||
|#

Complete description of all object fields can be found on the [Objects and Fields](../../entities.md) page.

## Error Handling

HTTP status: **400**

```json
{
    "error": "BOT_TYPE_NOT_ALLOWED",
    "error_description": "Method is available only for supervisor and personal bots"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `BOT_TOKEN_NOT_SPECIFIED` | Bot token not specified (botToken is required for webhook auth) | `botToken` is not specified. Required for webhook authorization ||
|| `BOT_ID_REQUIRED` | botId is required | `botId` is not specified or is less than 1 ||
|| `BOT_NOT_FOUND` | Bot not found | Bot not found ||
|| `BOT_OWNERSHIP_ERROR` | Bot was installed by another rest application | The bot was registered by another application or with a different `botToken` ||
|| `BOT_TYPE_NOT_ALLOWED` | Method is available only for supervisor and personal bots | The method is available only for bots of type `supervisor` and `personal` ||
|| `MESSAGE_NOT_FOUND` | MESSAGE_NOT_FOUND | Message not found ||
|| `ACCESS_DENIED` | ACCESS_DENIED | The bot is not a member of the private chat with this message. The bot can read messages of open chats without being a member ||
|| `MESSAGE_ACCESS_DENIED` | MESSAGE_ACCESS_DENIED | The bot is a member of the chat, but the message was sent before the bot was added, and the chat history is hidden from the bot ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [API Change Log for imbot.v2](../../change-log.md)
- [{#T}](./chat-message-get-context.md)
- [{#T}](./chat-message-send.md)
- [{#T}](./chat-message-update.md)
- [{#T}](../../index.md#bot-types)