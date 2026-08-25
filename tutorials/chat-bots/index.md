# How to Create a Chatbot That Replies With a List of Overdue Tasks

> Scope: [`imbot`, `task`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the whole scenario, you need the strictest of the listed permissions — the owner of the registered bot
>
> - [imbot.v2.Bot.register](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-register.md) — authenticated user
> - [imbot.v2.Chat.Message.send](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/messages/chat-message-send.md) — owner of the registered bot
> - [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) — any user

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Let's create the Reporter chatbot that notifies a user about their overdue tasks. The bot processes a single message — "what's burning" — and replies with a list of tasks where the user is the assignee and the deadline has already passed.

Verifiable result: after the application is installed, the Reporter bot appears in the chat list, and in response to the "what's burning" message it returns a list of tasks or a message that there are no overdue tasks.

Three objects take part in the scenario: an application with OAuth authorization, a registered chatbot, and the user's tasks.

{% note info "" %}

A chatbot is an [application](../../settings/app-installation/index.md) with OAuth authorization, not an incoming webhook. The application registers the bot, and Bitrix24 sends the bot events as HTTP requests to a public handler URL.

SDKs perform outgoing REST calls. Incoming events are received by your web server — Express, PHP, or Flask.

{% endnote %}

The scenario consists of four steps.

1. Register the bot on application installation using the [imbot.v2.Bot.register](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-register.md) method
2. Send a greeting on the [ONIMBOTV2JOINCHAT](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/events/events.md#onimbotv2joinchat) event using the [imbot.v2.Chat.Message.send](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/messages/chat-message-send.md) method
3. On the [ONIMBOTV2MESSAGEADD](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/events/events.md#onimbotv2messageadd) event, retrieve the tasks using the [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) method and reply using the [imbot.v2.Chat.Message.send](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/messages/chat-message-send.md) method
4. Clear the bot data on the [ONIMBOTV2DELETE](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/events/events.md#onimbotv2delete) event

The order of the steps is set by the platform: until the bot is registered, the `ONIMBOTV2*` events do not arrive, and the `dialogId` for the reply appears only in the event.

## Prepare the Application

The bot is registered on behalf of the application, so first prepare a local application and a handler.

1. Host the handler on a public HTTPS URL, for example `https://example.com/handler`
2. In the **Applications → Developer resources → Other → Local application** section, create an application and enable the "Uses only API" option
3. In the "Handler path" and "Initial installation path" fields, specify the same handler address
4. Grant the application the [`imbot`](../../api-reference/scopes/permissions.md) and [`task`](../../api-reference/scopes/permissions.md) permissions
5. Save the application and copy its `client_id` and `client_secret`

Prepare the values that you need to replace with your own:

#|
|| **Value** | **Where to take it from** ||
|| `B24_CLIENT_ID` | The `client_id` field in the local application card, in the form `local.xxxxxxxx.xxxxxxxx` ||
|| `B24_CLIENT_SECRET` | The `client_secret` field in the local application card ||
|| `HANDLER_URL` | Public HTTPS address of the event handler ||
|| `BOT_CODE` | Bot code, unique within the application, for example `overdue_tasks_bot` ||
|#

The `imbot.v2` methods are available in the cloud and in on-premise versions with an up-to-date API revision. If the bot runs on-premise, check the revision using the [imbot.v2.Revision.get](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/revision-get.md) method.

The application needs storage: a database, a file, or a secrets manager. In the examples it is represented by the `store` object with four operations:

- `saveAuth` and `loadAuth` — retain and read the authorization data by the `application_token` key
- `saveBot` and `removeBot` — retain and delete the ID of the registered bot

## Initializing the SDK Using Event Data {#sdk-init}

The application receives the authorization in the body of the installation event. Tokens live for one hour, so retain the whole pair and rebuild the SDK client from the storage on every subsequent event.

All the code below runs on the server only: `client_secret` and the tokens must not get into code that runs in the browser.

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    // npm install express @bitrix24/b24jssdk
    import { B24OAuth } from '@bitrix24/b24jssdk'

    const APP = {
        clientId: process.env.B24_CLIENT_ID,
        clientSecret: process.env.B24_CLIENT_SECRET,
    }

    // store — your storage: a database, a file, or a secrets manager

    // auth arrives in the body of the installation event
    function oauthParamsFromEvent(auth) {
        return {
            domain: auth.domain,
            clientEndpoint: auth.client_endpoint,
            serverEndpoint: auth.server_endpoint,
            accessToken: auth.access_token,
            refreshToken: auth.refresh_token,
            memberId: auth.member_id,
            applicationToken: auth.application_token,
            scope: auth.scope,
            status: auth.status,
            // expires does not arrive in every event — we calculate it from expires_in
            expires: Number(auth.expires ?? Math.floor(Date.now() / 1000) + Number(auth.expires_in)),
            expiresIn: Number(auth.expires_in),
            userId: Number(auth.user_id ?? 0),
        }
    }

    function makeClient(params) {
        const $b24 = new B24OAuth(params, APP)
        $b24.offClientSideWarning()
        // The SDK refreshes the tokens itself — we retain the whole new pair
        $b24.setCallbackRefreshAuth(async ({ b24OAuthParams }) => {
            await store.saveAuth(b24OAuthParams.applicationToken, b24OAuthParams)
        })
        return $b24
    }
    ```

- Python

    ```python
    # pip install b24pysdk flask
    import os

    from b24pysdk import BitrixApp, BitrixToken, Client

    APP = BitrixApp(
        client_id=os.environ["B24_CLIENT_ID"],
        client_secret=os.environ["B24_CLIENT_SECRET"],
    )

    # store — your storage: a database, a file, or a secrets manager


    # auth — the authorization dictionary from the body of the installation event
    def make_client(auth: dict) -> tuple:
        token = BitrixToken(
            domain=auth["domain"],
            auth_token=auth["access_token"],
            refresh_token=auth["refresh_token"],
            expires_in=int(auth["expires_in"]),
            bitrix_app=APP,
        )
        return Client(token), token
    ```


- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Core\Credentials\ApplicationProfile;
    use Bitrix24\SDK\Core\Credentials\AuthToken;
    use Bitrix24\SDK\Core\Credentials\DefaultOAuthServerUrl;
    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Monolog\Handler\StreamHandler;
    use Monolog\Logger;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    // $store — your storage: a database, a file, or a secrets manager
    // $auth — the authorization array from the body of the installation event
    function makeServiceBuilder(array $auth) {
        $appProfile = ApplicationProfile::initFromArray([
            'BITRIX24_PHP_SDK_APPLICATION_CLIENT_ID' => getenv('B24_CLIENT_ID'),
            'BITRIX24_PHP_SDK_APPLICATION_CLIENT_SECRET' => getenv('B24_CLIENT_SECRET'),
            'BITRIX24_PHP_SDK_APPLICATION_SCOPE' => 'imbot,task',
        ]);

        // expires does not arrive in every event — we calculate it from expires_in
        $authToken = new AuthToken(
            (string)$auth['access_token'],
            (string)$auth['refresh_token'],
            time() + (int)$auth['expires_in'],
            (int)$auth['expires_in'],
        );

        $log = new Logger('bot');
        $log->pushHandler(new StreamHandler('php://stdout'));

        return (new ServiceBuilderFactory(new EventDispatcher(), $log))
            ->init($appProfile, $authToken, (string)$auth['domain'], DefaultOAuthServerUrl::default());
    }
    ```
{% endlist %}

## Receive the Event in the Handler

A single endpoint receives all the events and routes them by the `event` field. The request body arrives as `application/x-www-form-urlencoded`, and the keys have the form `data[bot][id]` and `auth[application_token]`.

Verify the request authenticity by the `application_token` from the **top level** — `auth.application_token`, not by the token from `data.bot.auth`. Retain it during the installation and compare all subsequent events against it.

{% list tabs %}

- JS

    ```js
    import express from 'express'

    const app = express()
    app.use(express.urlencoded({ extended: true }))

    const HANDLER_URL = process.env.HANDLER_URL
    const BOT_CODE = 'overdue_tasks_bot'

    app.post('/handler', async (req, res) => {
        const event = req.body.event
        const data = req.body.data || {}
        const auth = req.body.auth || {}

        if (event !== 'ONAPPINSTALL' && !(await store.loadAuth(auth.application_token))) {
            return res.sendStatus(403)
        }

        try {
            if (event === 'ONAPPINSTALL') {
                await handleInstall(auth)
            } else if (event === 'ONIMBOTV2JOINCHAT') {
                await handleJoinChat(auth, data)
            } else if (event === 'ONIMBOTV2MESSAGEADD') {
                await handleMessage(auth, data)
            } else if (event === 'ONIMBOTV2DELETE') {
                await handleBotDelete(auth, data)
            }
        } catch (error) {
            console.error(event, error)
        }

        // The platform expects a 200 response, repeated delivery of an event is not guaranteed
        res.sendStatus(200)
    })

    app.listen(3000)
    ```

- Python

    ```python
    # handler.py
    import os
    import re

    from flask import Flask, request

    app = Flask(__name__)
    HANDLER_URL = os.environ["HANDLER_URL"]
    BOT_CODE = "overdue_tasks_bot"


    def unflatten(form) -> dict:
        """Assembles flat keys of the form data[bot][id] into a nested dictionary"""
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
        event = payload.get("event")
        data = payload.get("data", {})
        auth = payload.get("auth", {})

        if event != "ONAPPINSTALL" and store.load_auth(auth.get("application_token", "")) is None:
            return "", 403

        try:
            if event == "ONAPPINSTALL":
                handle_install(auth)
            elif event == "ONIMBOTV2JOINCHAT":
                handle_join_chat(auth, data)
            elif event == "ONIMBOTV2MESSAGEADD":
                handle_message(auth, data)
            elif event == "ONIMBOTV2DELETE":
                handle_bot_delete(auth, data)
        except Exception as error:
            app.logger.error("%s: %s", event, error)

        # The platform expects a 200 response, repeated delivery of an event is not guaranteed
        return "", 200
    ```


- PHP

    ```php
    <?php
    // handler.php
    require_once 'vendor/autoload.php';

    $event = (string)($_POST['event'] ?? '');
    $data = (array)($_POST['data'] ?? []);
    $auth = (array)($_POST['auth'] ?? []);

    $handlerUrl = getenv('HANDLER_URL');
    $botCode = 'overdue_tasks_bot';

    if ($event !== 'ONAPPINSTALL' && $store->loadAuth($auth['application_token'] ?? '') === null) {
        http_response_code(403);
        exit;
    }

    try {
        match ($event) {
            'ONAPPINSTALL' => handleInstall($auth),
            'ONIMBOTV2JOINCHAT' => handleJoinChat($auth, $data),
            'ONIMBOTV2MESSAGEADD' => handleMessage($auth, $data),
            'ONIMBOTV2DELETE' => handleBotDelete($auth, $data),
            default => null,
        };
    } catch (Throwable $exception) {
        error_log($event . ': ' . $exception->getMessage());
    }

    // The platform expects a 200 response, repeated delivery of an event is not guaranteed
    http_response_code(200);
    ```
{% endlist %}

Place the step functions from the examples below in the same file — the handler calls them by the event name.

## 1. Register the Bot on Application Installation

The [ONAPPINSTALL](../../api-reference/common/events/on-app-install.md) event arrives once after the installation. In it the application receives the tokens and the `application_token` — retain them and register the bot right away.

In [imbot.v2.Bot.register](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-register.md), the bot parameters are passed in the `fields` object:

- `code` — bot code, unique within the application
- `type` — the `bot` type suits private conversations and mentions in group chats
- `eventMode` and `webhookUrl` — the `webhook` mode enables event delivery to the handler address, a separate subscription through `event.bind` is not needed
- `properties.name` — the name the user sees in the chat list

With OAuth authorization, the `botToken` parameter is not needed: the bot is linked to the application through `client_id`.

{% list tabs %}

- JS

    ```js
    async function handleInstall(auth) {
        const params = oauthParamsFromEvent(auth)
        await store.saveAuth(params.applicationToken, params)

        const $b24 = makeClient(params)
        // There is no typed wrapper for imbot.v2, so the method is called through the SDK core
        const response = await $b24.actions.v2.call.make({
            method: 'imbot.v2.Bot.register',
            params: {
                fields: {
                    code: BOT_CODE,
                    type: 'bot',
                    eventMode: 'webhook',
                    webhookUrl: HANDLER_URL,
                    properties: {
                        name: 'Reporter',
                        workPosition: 'I report on overdue tasks',
                        color: 'aqua',
                    },
                },
            },
            requestId: 'imbot-v2-bot-register',
        })

        const { result } = response.getData()
        await store.saveBot(result.bot.id)
    }
    ```

- Python

    ```python
    def handle_install(auth: dict) -> None:
        store.save_auth(auth["application_token"], auth)

        _, token = make_client(auth)
        # There is no typed wrapper for imbot.v2, so the method is called through the SDK core
        response = token.call_method(
            "imbot.v2.Bot.register",
            {
                "fields": {
                    "code": BOT_CODE,
                    "type": "bot",
                    "eventMode": "webhook",
                    "webhookUrl": HANDLER_URL,
                    "properties": {
                        "name": "Reporter",
                        "workPosition": "I report on overdue tasks",
                        "color": "aqua",
                    },
                },
            },
        )
        store.save_bot(response["result"]["bot"]["id"])
    ```


- PHP

    ```php
    function handleInstall(array $auth): void
    {
        global $store, $handlerUrl, $botCode;

        $store->saveAuth((string)$auth['application_token'], $auth);

        $b24 = makeServiceBuilder($auth);
        // There is no typed wrapper for imbot.v2, so the method is called through the SDK core
        $result = $b24->core->call('imbot.v2.Bot.register', [
            'fields' => [
                'code' => $botCode,
                'type' => 'bot',
                'eventMode' => 'webhook',
                'webhookUrl' => $handlerUrl,
                'properties' => [
                    'name' => 'Reporter',
                    'workPosition' => 'I report on overdue tasks',
                    'color' => 'aqua',
                ],
            ],
        ])->getResponseData()->getResult();

        $store->saveBot((int)$result['bot']['id']);
    }
    ```
{% endlist %}

A successful response contains the bot object. Retain `result.bot.id` — you will need it if the application registers several bots.

```json
{
    "result": {
        "bot": {
            "id": 456,
            "code": "overdue_tasks_bot",
            "type": "bot",
            "eventMode": "webhook"
        }
    }
}
```

The method is idempotent: a repeated call with the same `fields.code` from the same application returns the existing bot and does not change its data. To change the properties of a registered bot, use [imbot.v2.Bot.update](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-update.md).

## 2. Send a Greeting When the Bot Is Added to a Chat

The [ONIMBOTV2JOINCHAT](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/events/events.md#onimbotv2joinchat) event arrives when a user opens a conversation with the bot or adds it to a chat. Take two values from the event:

- `data.bot.id` — the bot ID for the `botId` parameter
- `data.dialogId` — the dialog ID for the `dialogId` parameter

```json
{
    "event": "ONIMBOTV2JOINCHAT",
    "data": {
        "bot": {"id": 456, "code": "overdue_tasks_bot"},
        "dialogId": "chat5",
        "chat": {"id": 5, "dialogId": "chat5", "type": "chat"},
        "user": {"id": 1, "name": "Klaus Weber"}
    },
    "auth": {"domain": "example.bitrix24.com", "application_token": "51856fefc120afa4b628cc82d3935cce"}
}
```

In the greeting text we use the `[send=text]label[/send]` [BB code](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/messages/message-formatting.md): the user clicks the label, and the bot receives a message with that text.

{% list tabs %}

- JS

    ```js
    async function handleJoinChat(auth, data) {
        const $b24 = makeClient(await store.loadAuth(auth.application_token))

        await $b24.actions.v2.call.make({
            method: 'imbot.v2.Chat.Message.send',
            params: {
                botId: Number(data.bot.id),
                dialogId: data.dialogId,
                fields: {
                    message: 'Hello! I am Reporter. Ask [send=what\'s burning]What\'s burning?[/send]',
                },
            },
            requestId: 'imbot-v2-welcome',
        })
    }
    ```

- Python

    ```python
    def handle_join_chat(auth: dict, data: dict) -> None:
        _, token = make_client(store.load_auth(auth["application_token"]))

        token.call_method(
            "imbot.v2.Chat.Message.send",
            {
                "botId": int(data["bot"]["id"]),
                "dialogId": data["dialogId"],
                "fields": {
                    "message": "Hello! I am Reporter. Ask [send=what's burning]What's burning?[/send]",
                },
            },
        )
    ```


- PHP

    ```php
    function handleJoinChat(array $auth, array $data): void
    {
        global $store;

        $b24 = makeServiceBuilder($store->loadAuth((string)$auth['application_token']));

        $b24->core->call('imbot.v2.Chat.Message.send', [
            'botId' => (int)$data['bot']['id'],
            'dialogId' => (string)$data['dialogId'],
            'fields' => [
                'message' => 'Hello! I am Reporter. Ask [send=what\'s burning]What\'s burning?[/send]',
            ],
        ]);
    }
    ```
{% endlist %}

A successful response contains the ID of the sent message:

```json
{
    "result": {
        "id": 789,
        "uuidMap": {}
    }
}
```

## 3. Reply With a List of Overdue Tasks

The [ONIMBOTV2MESSAGEADD](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/events/events.md#onimbotv2messageadd) event arrives for every message a user sends to the bot. Take three values from the event:

- `data.message.text` — the message text, we use it to determine the command
- `data.user.id` — the message author, we substitute it into the `RESPONSIBLE_ID` filter
- `data.chat.dialogId` — the dialog to reply to

```json
{
    "event": "ONIMBOTV2MESSAGEADD",
    "data": {
        "bot": {"id": 456, "code": "overdue_tasks_bot"},
        "message": {"id": 790, "chatId": 5, "authorId": 1, "text": "what's burning"},
        "chat": {"id": 5, "dialogId": "chat5", "type": "chat"},
        "user": {"id": 1, "name": "Klaus Weber"}
    },
    "auth": {"domain": "example.bitrix24.com", "application_token": "51856fefc120afa4b628cc82d3935cce"}
}
```

{% note warning "" %}

In webhook mode, all scalar values arrive as strings: `"456"` instead of `456`, `"1"` or `"0"` instead of `true` and `false`. Cast the types explicitly, otherwise `botId` and `RESPONSIBLE_ID` will go into the request as strings.

{% endnote %}

Overdue tasks are retrieved with the [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) method and a filter:

- `RESPONSIBLE_ID` — the assignee, the ID of the message author
- `<DEADLINE` — the deadline earlier than the current moment
- `!REAL_STATUS` with the values `4` and `5` — the task is not completed and is not awaiting review. Bitrix24 does not treat tasks in these two statuses as overdue, even if the deadline has passed

In the response, the task fields are named in lowercase: `id`, `title`, `deadline`.

{% list tabs %}

- JS

    ```js
    async function handleMessage(auth, data) {
        const $b24 = makeClient(await store.loadAuth(auth.application_token))

        const text = (data.message.text || '').trim().toLowerCase()
        let message = 'I don\'t understand what you want to know. Ask [send=what\'s burning]What\'s burning?[/send]'

        if (text === 'what\'s burning') {
            const tasksResponse = await $b24.actions.v2.call.make({
                method: 'tasks.task.list',
                params: {
                    filter: {
                        RESPONSIBLE_ID: Number(data.user.id),
                        '<DEADLINE': new Date().toISOString(),
                        '!REAL_STATUS': [4, 5],
                    },
                    select: ['ID', 'TITLE', 'DEADLINE'],
                    order: { DEADLINE: 'asc' },
                },
                requestId: 'tasks-task-list',
            })

            const tasks = tasksResponse.getData().result.tasks || []
            message = tasks.length
                ? 'Overdue tasks:[br]' + tasks.map((task) => `- ${task.title}`).join('[br]')
                : 'You are working brilliantly! Not a single overdue task.'
        }

        await $b24.actions.v2.call.make({
            method: 'imbot.v2.Chat.Message.send',
            params: {
                botId: Number(data.bot.id),
                dialogId: data.chat.dialogId,
                fields: { message },
            },
            requestId: 'imbot-v2-reply',
        })
    }
    ```

- Python

    ```python
    from datetime import datetime, timezone


    def handle_message(auth: dict, data: dict) -> None:
        client, token = make_client(store.load_auth(auth["application_token"]))

        text = (data["message"].get("text") or "").strip().lower()
        message = "I don't understand what you want to know. Ask [send=what's burning]What's burning?[/send]"

        if text == "what's burning":
            tasks = client.tasks.task.list(
                filter={
                    "RESPONSIBLE_ID": int(data["user"]["id"]),
                    "<DEADLINE": datetime.now(timezone.utc).isoformat(),
                    "!REAL_STATUS": [4, 5],
                },
                select=["ID", "TITLE", "DEADLINE"],
                order={"DEADLINE": "asc"},
            ).response.result["tasks"]

            message = (
                "Overdue tasks:[br]" + "[br]".join(f"- {task['title']}" for task in tasks)
                if tasks
                else "You are working brilliantly! Not a single overdue task."
            )

        token.call_method(
            "imbot.v2.Chat.Message.send",
            {
                "botId": int(data["bot"]["id"]),
                "dialogId": data["chat"]["dialogId"],
                "fields": {"message": message},
            },
        )
    ```


- PHP

    ```php
    function handleMessage(array $auth, array $data): void
    {
        global $store;

        $b24 = makeServiceBuilder($store->loadAuth((string)$auth['application_token']));

        $text = mb_strtolower(trim((string)($data['message']['text'] ?? '')));
        $message = 'I don\'t understand what you want to know. Ask [send=what\'s burning]What\'s burning?[/send]';

        if ($text === 'what\'s burning') {
            $result = $b24->core->call('tasks.task.list', [
                'filter' => [
                    'RESPONSIBLE_ID' => (int)$data['user']['id'],
                    '<DEADLINE' => date('c'),
                    '!REAL_STATUS' => [4, 5],
                ],
                'select' => ['ID', 'TITLE', 'DEADLINE'],
                'order' => ['DEADLINE' => 'asc'],
            ])->getResponseData()->getResult();

            $tasks = $result['tasks'] ?? [];
            $message = $tasks
                ? 'Overdue tasks:[br]' . implode('[br]', array_map(
                    static fn(array $task): string => '- ' . $task['title'],
                    $tasks,
                ))
                : 'You are working brilliantly! Not a single overdue task.';
        }

        $b24->core->call('imbot.v2.Chat.Message.send', [
            'botId' => (int)$data['bot']['id'],
            'dialogId' => (string)$data['chat']['dialogId'],
            'fields' => ['message' => $message],
        ]);
    }
    ```
{% endlist %}

The `tasks.task.list` response is reduced to the fields that the bot uses:

```json
{
    "result": {
        "tasks": [
            {
                "id": "8017",
                "title": "Approve the estimate",
                "deadline": "2025-10-24T19:00:00+02:00"
            }
        ]
    }
}
```

If there are no tasks, `result.tasks` contains an empty array — the bot replies that there are no overdue tasks.

## 4. Clear the Data When the Bot Is Deleted

The [ONIMBOTV2DELETE](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/events/events.md#onimbotv2delete) event arrives when the bot has been deleted. This is the last event from it — release the data linked to the bot.

{% list tabs %}

- JS

    ```js
    async function handleBotDelete(auth, data) {
        await store.removeBot(Number(data.bot.id))
    }
    ```

- Python

    ```python
    def handle_bot_delete(auth: dict, data: dict) -> None:
        store.remove_bot(int(data["bot"]["id"]))
    ```


- PHP

    ```php
    function handleBotDelete(array $auth, array $data): void
    {
        global $store;

        $store->removeBot((int)$data['bot']['id']);
    }
    ```
{% endlist %}

## Check the Result

1. Install the application and make sure that the handler received `ONAPPINSTALL` and that the `imbot.v2.Bot.register` method returned `result.bot.id`
2. Check the registration with the [imbot.v2.Bot.list](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-list.md) method — the response must contain the bot with your `code`
3. Open a chat with the Reporter bot. The bot sends a greeting — this means that the `ONIMBOTV2JOINCHAT` event reached the handler
4. Send "what's burning". The bot replies with a list of tasks or with a message that there are no overdue tasks

Two signs confirm the success: the `imbot.v2.Chat.Message.send` method returned the `result.id` of the sent message, and the message appeared in the chat.

## Errors and Diagnostics

If a method returns an error, check the request data and the application permissions.

- `BOT_CODE_ALREADY_TAKEN` — the bot code is taken by another application, choose a different `fields.code` value
- `BOT_WEBHOOK_URL_REQUIRED` — `fields.webhookUrl` was not passed with `fields.eventMode = webhook`
- `BOT_INVALID_CALLBACK` — an invalid handler URL was passed in `fields.webhookUrl`
- `BOT_ID_REQUIRED` — `botId` was not passed in `imbot.v2.Chat.Message.send`, check `data.bot.id` in the event
- `BOT_NOT_FOUND` — the bot is deleted, repeat step 1 and register it again
- `ACCESS_DENIED` — the bot is not a chat member, check the `dialogId` from the event
- `EMPTY_MESSAGE` — an empty string arrived in `fields.message`
- `expired_token` — `access_token` has expired. The SDK refreshes it by `refresh_token` if the client is created with the `client_id` and `client_secret` of the application, retain the new pair in the storage

If there is no error but the bot stays silent, check the chain step by step:

- the bot did not appear in the chat list — the `ONAPPINSTALL` event did not reach the handler, or the application was not granted the `imbot` scope
- the bot appeared but does not reply — the handler does not receive the `ONIMBOTV2*` events: check `fields.eventMode` and `fields.webhookUrl` with the [imbot.v2.Bot.get](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-get.md) method
- the bot replies but the task list is always empty — the user of the token has no access to the assignee's tasks, call `tasks.task.list` with the same filter separately

## Important Notes

- The methods and events of the `imbot.*` branch are deprecated. Use `imbot.v2.*` for new bots, the migration order is described in the article [Migration from imbot to imbot.v2](../../api-reference/chat-bots/chat-bots-v2/migration.md)
- A bot of the `bot` type receives events in a private conversation and on the `@bot` mention in group chats. For the bot to see all the messages of a group chat, the `personal` or `supervisor` type is required — [Bot Types](../../api-reference/chat-bots/chat-bots-v2/index.md#bot-types)
- Tasks are returned within the permissions of the user whose token the application uses. An administrator sees all the tasks, a manager sees the tasks of their employees
- After the tokens are refreshed, retain the whole new pair, otherwise after a restart the application takes outdated values from the storage
- The platform does not guarantee repeated delivery of a webhook event. If you need a guarantee, register the bot with `eventMode: "fetch"` and collect the events with the [imbot.v2.Event.get](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/events/event-get.md) method
- To adapt the scenario to another task, change only step 3: the condition on the message text and the call that collects the data for the reply

## Continue Learning

- [{#T}](../../api-reference/chat-bots/chat-bots-v2/quick-start.md)
- [{#T}](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/bots/bot-register.md)
- [{#T}](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/messages/chat-message-send.md)
- [{#T}](../../api-reference/chat-bots/chat-bots-v2/imbot.v2/events/events.md)
- [{#T}](../../api-reference/tasks/tasks-task-list.md)
- [{#T}](./open-lines-bot.md)
