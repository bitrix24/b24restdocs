# How to Create a Chatbot for Open Channels

> Scope: [`imbot`, `imopenlines`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the whole scenario, the application needs the `imbot` and `imopenlines` permissions
>
> - [imbot.v2.Bot.register](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-register.md) - authenticated user
> - [imopenlines.bot.session.operator](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-operator.md) - any user
> - [imopenlines.bot.session.transfer](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-transfer.md) and [imopenlines.bot.session.finish](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-finish.md) - application user with a registered chatbot

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

An Open Channels chatbot receives customer requests, sends the first response, and transfers the conversation to an operator when needed. For this scenario, use the current [chatbots 2.0](../../api-reference/chat-bots/chat-bots-v2/index.md) platform.

The scenario consists of three steps.

1. Register the bot using [imbot.v2.Bot.register](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-register.md)
2. In the [ONIMBOTV2MESSAGEADD and ONIMBOTV2JOINCHAT](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/events/events.md) event handlers, check that the event came from an Open Channel
3. Manage the conversation using [imopenlines.bot.session.*](../../api-reference/imopenlines/openlines/chat-bots/index.md)

## Prepare Data

Before you start, create an application or an incoming webhook with the `imbot` and `imopenlines` permissions.

Prepare the values:

- `HANDLER_URL` - public HTTPS URL of the event handler
- `botToken` - bot token up to 40 characters long, if you use an incoming webhook
- `chatId` - Open Channel chat ID from the `data.message.chatId` or `data.chat.id` event field
- `operatorId` - ID of the employee to transfer the conversation to

For incoming webhook examples, save the webhook URL in the `B24_WEBHOOK_URL` environment variable and the bot token in `BOT_TOKEN`. For the Python example, split the webhook URL into the `B24_DOMAIN` domain and the `B24_WEBHOOK_TOKEN` path in the form `1/xxxxxxxxxxxxxxxx`.

Initialize the SDK before the first call.

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_WEBHOOK_URL)
    const botToken = process.env.BOT_TOKEN
    ```

- PHP

    ```php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Monolog\Handler\StreamHandler;
    use Monolog\Logger;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $b24 = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook(getenv('B24_WEBHOOK_URL'));

    $botToken = getenv('BOT_TOKEN');
    ```

- Python

    ```python
    # pip install b24pysdk
    import os
    from b24pysdk import BitrixWebhook, Client

    token = BitrixWebhook(
        domain=os.environ["B24_DOMAIN"],
        webhook_token=os.environ["B24_WEBHOOK_TOKEN"],
    )
    client = Client(token)
    bot_token = os.environ["BOT_TOKEN"]
    ```

{% endlist %}

## 1. Register the Bot for Open Channels

In [imbot.v2.Bot.register](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-register.md), bot parameters are passed in the `fields` object.

For hybrid mode, where the bot works in group chats, private chats, and Open Channels, pass `fields.type = bot` and `fields.isSupportOpenline = true`. If the bot is needed only for Open Channels, pass `fields.type = openline`.

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    // There is no typed wrapper for imbot.v2, so the method is called directly through the SDK core.
    await $b24.actions.v2.call.make({
        method: 'imbot.v2.Bot.register',
        params: {
            fields: {
                code: 'open_line_bot',
                botToken: botToken,
                type: 'bot',
                isSupportOpenline: true,
                eventMode: 'webhook',
                webhookUrl: HANDLER_URL,
                properties: {
                    name: 'Support line',
                    workPosition: 'First line',
                    color: 'GREEN',
                },
            },
        },
        requestId: 'imbot-v2-bot-register-ol',
    })
    ```

- PHP

    ```php
    // There is no typed wrapper for imbot.v2, so the method is called directly through the SDK core.
    $b24->core->call('imbot.v2.Bot.register', [
        'fields' => [
            'code' => 'open_line_bot',
            'botToken' => $botToken,
            'type' => 'bot',
            'isSupportOpenline' => true,
            'eventMode' => 'webhook',
            'webhookUrl' => $handlerUrl,
            'properties' => [
                'name' => 'Support line',
                'workPosition' => 'First line',
                'color' => 'GREEN',
            ],
        ],
    ]);
    ```

- Python

    ```python
    # There is no typed wrapper for imbot.v2, so the method is called directly through the SDK core.
    token.call_method(
        "imbot.v2.Bot.register",
        {
            "fields": {
                "code": "open_line_bot",
                "botToken": bot_token,
                "type": "bot",
                "isSupportOpenline": True,
                "eventMode": "webhook",
                "webhookUrl": HANDLER_URL,
                "properties": {
                    "name": "Support line",
                    "workPosition": "First line",
                    "color": "GREEN",
                },
            }
        },
    )
    ```

{% endlist %}

In a successful response, save `result.bot.id`. You will need it if the application works with several bots.

```json
{
    "result": {
        "bot": {
            "id": 456,
            "code": "open_line_bot",
            "type": "bot",
            "isSupportOpenline": true,
            "eventMode": "webhook"
        }
    }
}
```

## 2. Check the Chat Type in the Handler

In `ONIMBOTV2MESSAGEADD` and `ONIMBOTV2JOINCHAT` events, data arrives in the V2 format: field names use camelCase, and chat data is located in the `data.chat` object.

To process Open Channels messages separately, check the `data.chat.entityType` field. For Open Channels, it equals `LINES`.

Save `data.message.chatId` from the event to the `chatId` variable. This value is passed to the `CHAT_ID` parameter of session management methods.

```json
{
    "event": "ONIMBOTV2MESSAGEADD",
    "data": {
        "message": {
            "chatId": 112
        },
        "chat": {
            "id": 112,
            "entityType": "LINES"
        }
    }
}
```

{% list tabs %}

- JS

    ```js
    if (event.data.chat.entityType === 'LINES') {
        const chatId = event.data.message.chatId;
        // Message from an Open Channel
    }
    ```

- PHP

    ```php
    if (($event['data']['chat']['entityType'] ?? '') === 'LINES') {
        $chatId = (int)$event['data']['message']['chatId'];
        // Message from an Open Channel
    }
    ```

- Python

    ```python
    if event["data"]["chat"].get("entityType") == "LINES":
        chat_id = event["data"]["message"]["chatId"]
        # Message from an Open Channel
        ...
    ```

{% endlist %}

## 3. Manage the Session

With the `imopenlines` permission, the following commands for conversation management are available:

- [imopenlines.bot.session.operator](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-operator.md) - transfer to an available operator
- [imopenlines.bot.session.transfer](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-transfer.md) - transfer to a specific operator
- [imopenlines.bot.session.finish](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-finish.md) - finish the session

When calling `imopenlines.bot.session.transfer` and `imopenlines.bot.session.finish`, pass the same `CLIENT_ID` that was used when registering the bot. If the bot is registered through `imbot.v2.Bot.register` with `fields.botToken`, pass this value in `CLIENT_ID`.

{% list tabs %}

- JS

    ```js
    // Transfer conversation to an available operator
    await $b24.actions.v2.call.make({
        method: 'imopenlines.bot.session.operator',
        params: { CHAT_ID: chatId },
        requestId: 'session-operator',
    })

    // Transfer to a specific operator
    await $b24.actions.v2.call.make({
        method: 'imopenlines.bot.session.transfer',
        params: { CHAT_ID: chatId, USER_ID: operatorId, CLIENT_ID: botToken },
        requestId: 'session-transfer',
    })

    // End session
    await $b24.actions.v2.call.make({
        method: 'imopenlines.bot.session.finish',
        params: { CHAT_ID: chatId, CLIENT_ID: botToken },
        requestId: 'session-finish',
    })
    ```

- PHP

    ```php
    $b24->core->call('imopenlines.bot.session.operator', ['CHAT_ID' => $chatId]);
    $b24->core->call('imopenlines.bot.session.transfer', [
        'CHAT_ID' => $chatId,
        'USER_ID' => $operatorId,
        'CLIENT_ID' => $botToken,
    ]);
    $b24->core->call('imopenlines.bot.session.finish', [
        'CHAT_ID' => $chatId,
        'CLIENT_ID' => $botToken,
    ]);
    ```

- Python

    ```python
    client.imopenlines.bot.session.operator(chat_id=chat_id).response
    token.call_method(
        "imopenlines.bot.session.transfer",
        {"CHAT_ID": chat_id, "USER_ID": operator_id, "CLIENT_ID": bot_token},
    )
    token.call_method(
        "imopenlines.bot.session.finish",
        {"CHAT_ID": chat_id, "CLIENT_ID": bot_token},
    )
    ```

{% endlist %}

Successful response for each session management method:

```json
{
    "result": true
}
```

## Check the Result

Send a message to the connected Open Channel. The handler should receive `ONIMBOTV2MESSAGEADD` with `data.chat.entityType = LINES`.

To pass the conversation to an operator, call `imopenlines.bot.session.operator` or `imopenlines.bot.session.transfer` with `CHAT_ID` from the event. A successful response from these methods is `true`.

## Errors and Diagnostics

If a method returns an error, check the request data and application permissions.

- `BOT_TOKEN_NOT_SPECIFIED` - `fields.botToken` was not passed when authorizing through a webhook
- `BOT_INVALID_TYPE` - `fields.type` contains a value outside the list of allowed types
- `BOT_INVALID_EVENT_MODE` - `fields.eventMode` contains a value other than `fetch` or `webhook`
- `BOT_WEBHOOK_URL_REQUIRED` - `fields.webhookUrl` was not passed for webhook mode
- `CHAT_ID_EMPTY` - `CHAT_ID` was not passed or a value `<= 0` was passed
- `BOT_ID_ERROR` - no registered chatbot was found in the application

## Important Notes

- The `imbot.*` methods and events are deprecated. Use `imbot.v2.*` for new bots
- A bot registered through V1 receives `ONIMBOT*` events; a bot registered through V2 receives `ONIMBOTV2*` events
- For webhook mode, specify a public HTTPS URL in `fields.webhookUrl`
- Session management requires the `imopenlines` scope

## Continue Learning

- [{#T}](../../api-reference/chat-bots/chat-bots-v2/migration.md)
- [{#T}](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-register.md)
- [{#T}](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/events/events.md)
- [{#T}](../../api-reference/imopenlines/openlines/chat-bots/index.md)
