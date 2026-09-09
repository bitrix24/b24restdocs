# How to Create a Chatbot for Open Channels

> Scope: [`imbot`, `imopenlines`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the whole scenario, you need the strictest of the listed permissions — the user of the application or webhook that the chatbot is registered through
>
> - [imbot.v2.Bot.register](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-register.md) — authenticated user
> - [imopenlines.bot.session.message.send](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-message-send.md) — any user
> - [imopenlines.bot.session.operator](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-operator.md) — any user
> - [imopenlines.bot.session.transfer](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-transfer.md) and [imopenlines.bot.session.finish](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-finish.md) — application user with a registered chatbot

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

An Open Channels chatbot receives customer requests, sends the first response, and transfers the conversation to an operator when needed. For this scenario, use the current [chatbots 2.0](../../api-reference/chat-bots/chat-bots-v2/index.md) platform.

Verifiable result: the customer writes to an Open Channel, the bot replies with an automatic message, and on the "operator" keyword it transfers the conversation to an employee. Both the bot reply and the connected operator are visible in the channel chat.

Three objects take part in the scenario:

- an incoming webhook with the `imbot` and `imopenlines` permissions
- a registered chatbot with Open Channels support
- an Open Channel this bot is connected to

{% note info "" %}

SDKs perform outgoing REST calls only. Incoming events are received by your web server — an Express or Flask application, or a plain PHP script.

{% endnote %}

The scenario consists of four steps.

1. Register a bot with Open Channels support using the [imbot.v2.Bot.register](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-register.md) method and connect it to the channel
2. Receive the [ONIMBOTV2MESSAGEADD](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/events/events.md#onimbotv2messageadd) event in the handler and check that the chat belongs to an Open Channel
3. Reply to the customer using the [imopenlines.bot.session.message.send](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-message-send.md) method
4. Transfer the conversation to an operator or finish the session using the [imopenlines.bot.session.operator](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-operator.md), [imopenlines.bot.session.transfer](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-transfer.md), and [imopenlines.bot.session.finish](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-finish.md) methods

The order is set by the platform: the chat identifier for session management appears only in the event, and the events arrive only after the bot is registered.

## Prepare the Data

The examples on this page work through an incoming webhook: it does not require installing an application, and the bot is registered in Bitrix24 right away. The differences of the scenario for an application with OAuth authorization are collected in the [Important Notes](#important) block.

1. Create an incoming webhook with the `imbot` and `imopenlines` permissions
2. Host the event handler on a public HTTPS URL, for example `https://example.com/handler`
3. Set up an Open Channel and connect a communication channel to it — the website live chat or a messenger

Only a Bitrix24 administrator can create a webhook and set up an Open Channel.

Prepare the values that you need to replace with your own:

#|
|| **Value** | **Where to take it from** ||
|| `B24_WEBHOOK_URL` | Incoming webhook URL in the form `https://example.bitrix24.com/rest/1/xxxxxxxxxxxxxxxx/` ||
|| `BOT_TOKEN` | Come up with a unique bot token up to 40 characters long. It is bound to the bot during the registration ||
|| `HANDLER_URL` | Public HTTPS address of the event handler. In the JS and Python examples the handler listens on the `/handler` path, in the PHP example it is the `handler.php` file ||
|| `OPERATOR_ID` | ID of the employee the bot transfers the conversation to. You can see it in the URL of the employee profile or in the response of the [user.get](../../api-reference/user/user-get.md) and [user.search](../../api-reference/user/user-search.md) methods — these two methods need a separate `user` permission, the scenario itself does not require it ||
|#

You do not need to substitute the chat identifier: it arrives in the `data.chat.id` field of the event and is passed to the `CHAT_ID` parameter of the session management methods.

{% note warning "" %}

The incoming webhook URL and the bot token are secrets. The URL grants the whole access of the webhook, and the token allows managing sessions on behalf of the bot. Retain both values in the server environment variables and do not place them in code that runs in the browser.

{% endnote %}

For the Python example, split the webhook URL into the `B24_DOMAIN` domain and the `B24_WEBHOOK_TOKEN` path in the form `1/xxxxxxxxxxxxxxxx`.

Initialize the SDK and read the prepared values before the first call.

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    // npm install express @bitrix24/b24jssdk
    import { B24Hook, Text } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_WEBHOOK_URL)

    const botToken = process.env.BOT_TOKEN
    const handlerUrl = process.env.HANDLER_URL
    const operatorId = Number(process.env.OPERATOR_ID)

    // The SDK does not throw an exception on a REST error, so we check the isSuccess flag
    async function call(method, params) {
        const response = await $b24.actions.v2.call.make({
            method,
            params,
            requestId: Text.getUuidRfc4122(),
        })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }
    ```

- Python

    ```python
    # pip install b24pysdk flask
    import os

    from b24pysdk import BitrixWebhook, Client

    token = BitrixWebhook(
        domain=os.environ["B24_DOMAIN"],
        webhook_token=os.environ["B24_WEBHOOK_TOKEN"],
    )
    client = Client(token)

    bot_token = os.environ["BOT_TOKEN"]
    handler_url = os.environ["HANDLER_URL"]
    operator_id = int(os.environ["OPERATOR_ID"])
    ```


- PHP

    ```php
    <?php
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
    $handlerUrl = getenv('HANDLER_URL');
    $operatorId = (int)getenv('OPERATOR_ID');
    ```
{% endlist %}

This code is needed both by the one-time registration script from step 1 and by the permanently running handler from steps 2–4. Keep it in both files or move it to a shared module.

## 1. Register the Bot and Connect It to the Channel

In [imbot.v2.Bot.register](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-register.md), the bot parameters are passed in the `fields` object:

- `code` — bot code, unique within the webhook or the application
- `botToken` — bot token, required when authorizing through an incoming webhook
- `type` — bot type
- `isSupportOpenline` — Open Channels support
- `eventMode` — event delivery mode, the `webhook` value sends the events to the handler URL, and a separate subscription with the `event.bind` method is not needed
- `webhookUrl` — event handler URL
- `properties` — bot profile: the `name` name, the `workPosition` job title, and the `color` avatar color

Choose the type according to your task. For a hybrid bot that works in group chats, private conversations, and Open Channels, pass `type = bot` and `isSupportOpenline = true`. If the bot is needed only for Open Channels, pass `type = openline` — the channel support turns on by itself. Without it, the bot does not receive the events from the channel chat.

The registration is performed once. In the Python and PHP examples the SDKs throw an exception themselves if the method returns an error, and in JS the calls go through the `call` function from the preparation block — it checks the `isSuccess` flag.

{% list tabs %}

- JS

    ```js
    // There is no typed wrapper for imbot.v2, so the method is called directly through the SDK core.
    const result = await call('imbot.v2.Bot.register', {
        fields: {
            code: 'open_line_bot',
            botToken: botToken,
            type: 'bot',
            isSupportOpenline: true,
            eventMode: 'webhook',
            webhookUrl: handlerUrl,
            properties: {
                name: 'Support line',
                workPosition: 'First line',
                color: 'green',
            },
        },
    })

    const botId = Number(result.bot.id)
    ```

- Python

    ```python
    # There is no typed wrapper for imbot.v2, so the method is called directly through the SDK core.
    response = token.call_method(
        "imbot.v2.Bot.register",
        {
            "fields": {
                "code": "open_line_bot",
                "botToken": bot_token,
                "type": "bot",
                "isSupportOpenline": True,
                "eventMode": "webhook",
                "webhookUrl": handler_url,
                "properties": {
                    "name": "Support line",
                    "workPosition": "First line",
                    "color": "green",
                },
            }
        },
    )

    bot_id = int(response["result"]["bot"]["id"])
    ```


- PHP

    ```php
    // There is no typed wrapper for imbot.v2, so the method is called directly through the SDK core.
    $result = $b24->core->call('imbot.v2.Bot.register', [
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
                'color' => 'green',
            ],
        ],
    ])->getResponseData()->getResult();

    $botId = (int)$result['bot']['id'];
    ```
{% endlist %}

In a successful response, retain `result.bot.id`: you need it when the webhook manages several bots. The `isSupportOpenline` field confirms that the bot is accepted as an Open Channels bot. The example is shortened, the full response format is on the [imbot.v2.Bot.register](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-register.md) page.

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

The method is idempotent: a repeated call with the same `fields.code` returns the existing bot and does not change its data. To change the properties of a registered bot or the handler URL, use [imbot.v2.Bot.update](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-update.md).

After the registration, connect the bot to the channel: open *Contact Center > Open Channels*, edit the required channel, and specify the bot in the chatbot settings block. The moment of connection is set there as well — for example, right at the first customer request.

{% note warning "" %}

Until the bot is connected to the channel, it does not join the session chat. The registration method still completes successfully, but the `ONIMBOTV2*` events from the Open Channel do not reach the handler.

{% endnote %}

## 2. Receive the Event and Check That the Chat Belongs to a Channel

Bitrix24 sends the bot events as a POST request to the URL from `fields.webhookUrl`. The request body arrives as `application/x-www-form-urlencoded`, and the keys have the form `data[chat][entityType]` and `auth[application_token]`. All scalar values are passed as strings, so cast the types explicitly.

The handler receives all the bot events on a single URL and routes them by the `event` field. The bot receives the events from all of its chats, so check the `data.chat.entityType` field: for Open Channels chats it equals `LINES`.

Take two values from the [ONIMBOTV2MESSAGEADD](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/events/events.md#onimbotv2messageadd) event:

- `data.chat.id` — chat identifier, it has to be passed to the `CHAT_ID` parameter of the session management methods
- `data.message.text` — text of the customer message, the bot chooses the reply by it

Verify the request authenticity by the `auth.application_token` from the top level, not by the token from `data.bot.auth`. For a bot registered through an incoming webhook, `auth.application_token` equals the `custom` string glued together with `botToken`, without a separator.

```json
{
    "event": "ONIMBOTV2MESSAGEADD",
    "data": {
        "bot": {"id": 456, "code": "open_line_bot"},
        "message": {"id": 790, "chatId": 112, "authorId": 27, "text": "I need an operator"},
        "chat": {"id": 112, "dialogId": "chat112", "type": "lines", "entityType": "LINES"},
        "user": {"id": 27, "name": "Customer"}
    },
    "auth": {"domain": "example.bitrix24.com", "application_token": "custommy_bot_token"}
}
```

The example shows the event after the request body is parsed. In the request itself, the same data arrives as flat keys: `data[chat][entityType]=LINES`, `data[chat][id]=112`.

Place the `sendReply` and `handleLinesMessage` functions from steps 3 and 4 in the same file — the handler calls them by name.

{% list tabs %}

- JS

    ```js
    // SDK initialization and the variables — from the "Prepare the Data" block
    import express from 'express'

    const app = express()
    app.use(express.urlencoded({ extended: true }))

    app.post('/handler', async (req, res) => {
        const data = req.body.data || {}
        const auth = req.body.auth || {}

        if (auth.application_token !== `custom${botToken}`) {
            return res.sendStatus(403)
        }

        if (req.body.event === 'ONIMBOTV2MESSAGEADD' && data.chat?.entityType === 'LINES') {
            const chatId = Number(data.chat.id)
            const text = String(data.message?.text ?? '').trim().toLowerCase()

            try {
                await handleLinesMessage(chatId, text)
            } catch (error) {
                console.error(error)
            }
        }

        // The platform expects a 200 response, repeated delivery of an event is not guaranteed
        res.sendStatus(200)
    })

    app.listen(3000)
    ```

- Python

    ```python
    # SDK initialization and the variables — from the "Prepare the Data" block
    import re

    from flask import Flask, request

    app = Flask(__name__)


    def unflatten(form) -> dict:
        """Assembles flat keys of the form data[chat][entityType] into a nested dictionary"""
        result = {}
        for key, value in form.items():
            path = re.findall(r"[^\[\]]+", key)
            node = result
            for part in path[:-1]:
                node = node.setdefault(part, {})
            node[path[-1]] = value
        return result


    @app.post("/handler")
    def handler():
        payload = unflatten(request.form)
        data = payload.get("data", {})
        auth = payload.get("auth", {})

        if auth.get("application_token") != f"custom{bot_token}":
            return "", 403

        chat = data.get("chat", {})
        if payload.get("event") == "ONIMBOTV2MESSAGEADD" and chat.get("entityType") == "LINES":
            chat_id = int(chat["id"])
            text = (data.get("message", {}).get("text") or "").strip().lower()

            try:
                handle_lines_message(chat_id, text)
            except Exception as error:
                app.logger.error("%s", error)

        # The platform expects a 200 response, repeated delivery of an event is not guaranteed
        return "", 200


    if __name__ == "__main__":
        app.run(port=3000)
    ```


- PHP

    ```php
    // Continuation of handler.php: SDK initialization and the variables — from the "Prepare the Data" block
    $event = (string)($_POST['event'] ?? '');
    $data = (array)($_POST['data'] ?? []);
    $auth = (array)($_POST['auth'] ?? []);

    if (($auth['application_token'] ?? '') !== 'custom' . $botToken) {
        http_response_code(403);
        exit;
    }

    if ($event === 'ONIMBOTV2MESSAGEADD' && ($data['chat']['entityType'] ?? '') === 'LINES') {
        $chatId = (int)($data['chat']['id'] ?? 0);
        $text = mb_strtolower(trim((string)($data['message']['text'] ?? '')));

        try {
            handleLinesMessage($chatId, $text);
        } catch (Throwable $exception) {
            error_log($exception->getMessage());
        }
    }

    // The platform expects a 200 response, repeated delivery of an event is not guaranteed
    http_response_code(200);
    ```
{% endlist %}

## 3. Reply to the Customer

A message on behalf of the bot in the current channel session is sent by the [imopenlines.bot.session.message.send](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-message-send.md) method. Parameters:

- `CHAT_ID` — chat identifier, the `data.chat.id` value from the event
- `NAME` — reply mode: `DEFAULT` sends the text from `MESSAGE`, `WELCOME` sends the greeting from the Open Channel settings and ignores `MESSAGE`
- `MESSAGE` — reply text for the `DEFAULT` mode. With an empty text, no message is added to the chat

The method works with the current channel session, it does not need `CLIENT_ID`. Wrap the call in a separate function — it comes in handy in step 4.

{% list tabs %}

- JS

    ```js
    async function sendReply(chatId, message) {
        await call('imopenlines.bot.session.message.send', {
            CHAT_ID: chatId,
            NAME: 'DEFAULT',
            MESSAGE: message,
        })
    }
    ```

- Python

    ```python
    def send_reply(chat_id: int, message: str) -> None:
        client.imopenlines.bot.session.message.send(
            chat_id=chat_id,
            message=message,
            name="DEFAULT",
        ).response
    ```


- PHP

    ```php
    function sendReply(int $chatId, string $message): void
    {
        global $b24;

        $b24->core->call('imopenlines.bot.session.message.send', [
            'CHAT_ID' => $chatId,
            'NAME' => 'DEFAULT',
            'MESSAGE' => $message,
        ]);
    }
    ```
{% endlist %}

The `true` response confirms that the call is executed. The method does not return a confirmation that the message appeared in the chat, so check the result in the channel chat.

```json
{
    "result": true
}
```

## 4. Transfer the Conversation to an Operator or Finish the Session

With the `imopenlines` permission, three session management methods are available to the bot:

- [imopenlines.bot.session.operator](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-operator.md) — transfer the conversation to the first available operator of the channel, only `CHAT_ID` is needed
- [imopenlines.bot.session.transfer](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-transfer.md) — transfer to a specific employee in the `USER_ID` parameter or to a queue in the `QUEUE_ID` parameter, only one assignment at a time
- [imopenlines.bot.session.finish](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-finish.md) — finish the session

The `imopenlines.bot.session.transfer` and `imopenlines.bot.session.finish` methods act on behalf of the bot, so the webhook passes in the `CLIENT_ID` parameter the same `botToken` that was used during the registration. The `imopenlines.bot.session.operator` method has no `CLIENT_ID` parameter.

The `LEAVE` flag in the `imopenlines.bot.session.transfer` method defines whether the bot stays in the chat: `Y` — the bot leaves immediately, `N` — it stays until the transfer is confirmed. The default value is `N`.

Collect the reply branches into the `handleLinesMessage` function that the handler from step 2 calls. The example handles three keywords:

- "operator" — transfer the conversation to the first available employee of the channel
- "manager" — transfer the conversation to the employee from `OPERATOR_ID`
- "thank you" — say goodbye and finish the session

The bot replies to all the other messages with a hint.

{% list tabs %}

- JS

    ```js
    async function handleLinesMessage(chatId, text) {
        if (text.includes('operator')) {
            await call('imopenlines.bot.session.operator', { CHAT_ID: chatId })
            return
        }

        if (text.includes('manager')) {
            await call('imopenlines.bot.session.transfer', {
                CHAT_ID: chatId,
                USER_ID: operatorId,
                LEAVE: 'Y',
                CLIENT_ID: botToken,
            })
            return
        }

        if (text === 'thank you') {
            await sendReply(chatId, 'Happy to help! Feel free to reach out again')
            await call('imopenlines.bot.session.finish', {
                CHAT_ID: chatId,
                CLIENT_ID: botToken,
            })
            return
        }

        await sendReply(chatId, 'Hello! Describe your question or type "operator" to connect an employee')
    }
    ```

- Python

    ```python
    def handle_lines_message(chat_id: int, text: str) -> None:
        if "operator" in text:
            client.imopenlines.bot.session.operator(chat_id=chat_id).response
            return

        if "manager" in text:
            # The typed wrapper does not accept CLIENT_ID, so the method is called through the SDK core
            token.call_method(
                "imopenlines.bot.session.transfer",
                {
                    "CHAT_ID": chat_id,
                    "USER_ID": operator_id,
                    "LEAVE": "Y",
                    "CLIENT_ID": bot_token,
                },
            )
            return

        if text == "thank you":
            send_reply(chat_id, "Happy to help! Feel free to reach out again")
            token.call_method(
                "imopenlines.bot.session.finish",
                {"CHAT_ID": chat_id, "CLIENT_ID": bot_token},
            )
            return

        send_reply(chat_id, "Hello! Describe your question or type 'operator' to connect an employee")
    ```


- PHP

    ```php
    function handleLinesMessage(int $chatId, string $text): void
    {
        global $b24, $botToken, $operatorId;

        if (str_contains($text, 'operator')) {
            $b24->core->call('imopenlines.bot.session.operator', ['CHAT_ID' => $chatId]);

            return;
        }

        if (str_contains($text, 'manager')) {
            $b24->core->call('imopenlines.bot.session.transfer', [
                'CHAT_ID' => $chatId,
                'USER_ID' => $operatorId,
                'LEAVE' => 'Y',
                'CLIENT_ID' => $botToken,
            ]);

            return;
        }

        if ($text === 'thank you') {
            sendReply($chatId, 'Happy to help! Feel free to reach out again');
            $b24->core->call('imopenlines.bot.session.finish', [
                'CHAT_ID' => $chatId,
                'CLIENT_ID' => $botToken,
            ]);

            return;
        }

        sendReply($chatId, 'Hello! Describe your question or type "operator" to connect an employee');
    }
    ```
{% endlist %}

Successful response of each session management method:

```json
{
    "result": true
}
```

## Check the Result

1. Check the registration with the [imbot.v2.Bot.list](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-list.md) method and the `botToken` parameter — the `result.bots` array contains the bot with your `code`, its `isSupportOpenline` equals `true`, and `eventMode` equals `webhook`
2. Write to the channel connected to the Open Channel. The handler receives `ONIMBOTV2MESSAGEADD`, in which `data.chat.entityType` equals `LINES`, and the bot replies with the text from step 3
3. Write "operator". The `imopenlines.bot.session.operator` method returns `true`, and an employee of the channel joins the conversation

The whole conversation is visible in the *Contact Center > Open Channels* section: the session history contains both the bot replies and the moment of the transfer to the operator.

## Errors and Diagnostics

If a method returns an error, check the request data and the webhook permissions.

- `BOT_TOKEN_NOT_SPECIFIED` — `fields.botToken` was not passed, it is required when authorizing through a webhook
- `BOT_TOKEN_INVALID_LENGTH` — the bot token is longer than 40 characters, shorten the `BOT_TOKEN` value
- `BOT_WEBHOOK_URL_REQUIRED` — `fields.webhookUrl` was not passed for webhook mode, specify the handler URL
- `BOT_INVALID_CALLBACK` — an invalid URL was passed in `fields.webhookUrl`, check the `https` scheme and the domain name
- `BOT_CODE_ALREADY_TAKEN` — the bot code is taken, choose a different `fields.code` value
- `CHAT_ID_EMPTY` — `CHAT_ID` was not passed or a value `<= 0` was passed, take `data.chat.id` from the event
- `USER_ID_EMPTY` — an empty `USER_ID` or a value `<= 0` was passed to `imopenlines.bot.session.transfer`. This happens when the `OPERATOR_ID` variable is not set in the environment
- `BOT_ID_ERROR` — a value with no registered bot behind it was passed in `CLIENT_ID`, compare it with `fields.botToken` from step 1
- `ACCESS_DENIED` — the `CLIENT_ID` parameter was not passed at all, or the webhook has no `imopenlines` permission, check both conditions
- `WRONG_CHAT` — the conversation is already handled by an operator, not by the bot, there is no need to transfer the session again
- `OPERATOR_WRONG` — the conversation cannot be transferred to the specified employee or queue, check `USER_ID`

If there is no error but the bot stays silent, check the chain step by step:

- the events do not reach the handler — the bot is not connected to the channel in the Open Channel settings, or `fields.eventMode` still holds the `fetch` value. Check the bot with the `imbot.v2.Bot.list` method and repeat step 1
- the events arrive but the handler responds with `403` — the `BOT_TOKEN` value in the server environment does not match the registration token, compare it with `fields.botToken` from step 1
- the events arrive but the condition does not fire — compare `data.chat.entityType` with the `LINES` string and keep in mind that in webhook mode all the scalars arrive as strings
- the handler responds with something other than `200` — the platform does not guarantee repeated delivery of an event, and the conversation stays without a reply

## Important Notes {#important}

- The methods and events of the `imbot.*` branch are deprecated. Use `imbot.v2.*` for new bots, the migration order is described in the article [Migration from imbot to imbot.v2](../../api-reference/chat-bots/chat-bots-v2/migration.md)
- A bot registered with the `imbot.*` methods receives `ONIMBOT*` events, and a bot from `imbot.v2.*` receives `ONIMBOTV2*` events
- In an Open Channel chat, the bot receives all the customer messages without the `@bot` mention, unlike in group chats
- In an application with OAuth authorization, `fields.botToken` during the registration and `CLIENT_ID` during the session management are not needed: the bot is bound to the application through `client_id`. At the same time, the events do not arrive until the application [completes the installation](../../settings/app-installation/installation-finish.md)
- To adapt the scenario to your task, change only the `handleLinesMessage` function: the conditions on the text, the reply texts, and the way the conversation is transferred
- To route the requests to a queue, pass to `imopenlines.bot.session.transfer` the `QUEUE_ID` parameter with the value of the `ID` field from the response of the [imopenlines.config.list.get](../../api-reference/imopenlines/openlines/imopenlines-config-list-get.md) method

## Continue Learning

- [{#T}](../../api-reference/chat-bots/chat-bots-v2/migration.md)
- [{#T}](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-register.md)
- [{#T}](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/events/events.md)
- [{#T}](../../api-reference/imopenlines/openlines/chat-bots/index.md)
- [{#T}](./index.md)
