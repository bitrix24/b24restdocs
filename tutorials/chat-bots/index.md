# Chatbot Creation Example

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

As an example, we will create a chatbot that notifies a user about their overdue tasks. The bot processes a single command — "What's urgent?" — and returns a list of overdue tasks.

{% note info "" %}

A chatbot is an [application](../../settings/app-installation/index.md) with OAuth authorization, not an incoming webhook. Bitrix24 sends events (`ONAPPINSTALL`, `ONIMBOTMESSAGEADD`, `ONIMBOTJOINCHAT`, `ONIMBOTDELETE`) to the application via HTTP requests to a single public handler URL. The application needs permissions (scopes): `imbot` for bot registration, `im` for sending messages, and `task` and `task_extended` for task access.

SDKs perform outgoing REST calls. Your web server (Express, PHP, Flask) receives incoming events, and authorization (`access_token`, `domain`) is retrieved from the event request body.

{% endnote %}

## How the Bot Works

1. `ONAPPINSTALL` — during installation, we register the bot using the [imbot.register](../../api-reference/chat-bots/outdated/bots/imbot-register.md) method, specifying the event handler URLs.
2. `ONIMBOTJOINCHAT` — when a conversation is opened, we send a greeting using the [imbot.message.add](../../api-reference/chat-bots/outdated/messages/imbot-message-add.md) method.
3. `ONIMBOTMESSAGEADD` — upon receiving the "What's urgent?" message, we retrieve overdue tasks via [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) and respond.
4. `ONIMBOTDELETE` — when the bot is deleted, we clear the saved configuration.

{% note warning "" %}

The classic example uses the deprecated method `task.item.list`. In new integrations, use [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) — it is used in the examples below.

{% endnote %}

## Initializing the SDK Using Event Data

The application receives authorization in the body of every event. We use this to build the SDK client for outgoing calls.

{% list tabs %}

- JS

    ```js
    // npm install express @bitrix24/b24jssdk
    import { B24OAuth } from '@bitrix24/b24jssdk'

    const APP = { clientId: 'local.xxxxxxxx.xxxxxxxx', clientSecret: 'yyyyyyyy' }

    // auth arrives in the event body: { domain, access_token, refresh_token, application_token, ... }
    function makeClient(auth) {
        const $b24 = new B24OAuth({
            domain: auth.domain,
            accessToken: auth.access_token,
            refreshToken: auth.refresh_token,
            memberId: auth.member_id,
        }, APP)
        $b24.offClientSideWarning()
        return $b24
    }
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Bitrix24\SDK\Core\Credentials\ApplicationProfile;
    use Bitrix24\SDK\Core\Credentials\AuthToken;
    use Bitrix24\SDK\Core\Credentials\DefaultOAuthServerUrl;
    use Symfony\Component\HttpFoundation\Request;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    function makeServiceBuilder(Request $request) {
        $appProfile = ApplicationProfile::initFromArray([
            'BITRIX24_PHP_SDK_APPLICATION_CLIENT_ID' => 'local.xxxxxxxx.xxxxxxxx',
            'BITRIX24_PHP_SDK_APPLICATION_CLIENT_SECRET' => 'yyyyyyyy',
            'BITRIX24_PHP_SDK_APPLICATION_SCOPE' => 'imbot,im,task',
        ]);
        // The application token is taken from the event body
        $authToken = AuthToken::initFromEventRequest($request);
        $domain = (string)$request->request->all('auth')['domain'];

        $log = new Logger('bot');
        $log->pushHandler(new StreamHandler('php://stdout'));
        return (new ServiceBuilderFactory(new EventDispatcher(), $log))
            ->init($appProfile, $authToken, $domain, DefaultOAuthServerUrl::default());
    }
    ```

- Python

    ```python
    # pip install b24pysdk flask
    from b24pysdk import Client, BitrixApp, BitrixToken

    APP = BitrixApp(client_id="local.xxxxxxxx.xxxxxxxx", client_secret="yyyyyyyy")

    def make_client(auth: dict) -> tuple:
        token = BitrixToken(
            domain=auth["domain"],
            auth_token=auth["access_token"],
            refresh_token=auth.get("refresh_token", ""),
            bitrix_app=APP,
        )
        return Client(token), token
    ```

{% endlist %}

## Event Handler

A single endpoint receives all events and routes them based on the `event` field.

{% list tabs %}

- JS

    ```js
    import express from 'express'

    const app = express()
    app.use(express.urlencoded({ extended: true }))

    const HANDLER_URL = 'https://your-domain.example/handler'

    app.post('/handler', async (req, res) => {
        const event = req.body.event
        const auth = req.body.auth || {}
        const data = req.body.data || {}
        const $b24 = makeClient(auth)

        try {
            if (event === 'ONAPPINSTALL') {
                await $b24.actions.v2.call.make({
                    method: 'imbot.register',
                    params: {
                        CODE: 'ReportBot',
                        TYPE: 'B',
                        EVENT_MESSAGE_ADD: HANDLER_URL,
                        EVENT_WELCOME_MESSAGE: HANDLER_URL,
                        EVENT_BOT_DELETE: HANDLER_URL,
                        PROPERTIES: { NAME: 'Berichtender', COLOR: 'AQUA', WORK_POSITION: 'Reporting on affairs' },
                    },
                    requestId: 'imbot-register',
                })
            } else if (event === 'ONIMBOTJOINCHAT') {
                await $b24.actions.v2.call.make({
                    method: 'imbot.message.add',
                    params: {
                        DIALOG_ID: data.PARAMS.DIALOG_ID,
                        MESSAGE: 'Hello! I am Berichtender. Ask [send=what\'s burning]What\'s burning?[/send]',
                    },
                    requestId: 'welcome',
                })
            } else if (event === 'ONIMBOTMESSAGEADD') {
                const text = (data.PARAMS.MESSAGE || '').toLowerCase()
                const userId = data.PARAMS.FROM_USER_ID
                let message = 'I don\'t understand what you want to know.'
                if (text === 'what\'s burning') {
                    const tasksResp = await $b24.actions.v2.call.make({
                        method: 'tasks.task.list',
                        params: {
                            filter: { RESPONSIBLE_ID: userId, '<DEADLINE': new Date().toISOString() },
                            select: ['ID', 'TITLE', 'DEADLINE'],
                        },
                        requestId: 'tasks',
                    })
                    const tasks = tasksResp.getData().result.tasks || []
                    message = tasks.length
                        ? 'Overdue tasks:\n' + tasks.map((t) => `• ${t.title}`).join('\n')
                        : 'You\'re working brilliantly! Not a single overdue task.'
                }
                await $b24.actions.v2.call.make({
                    method: 'imbot.message.add',
                    params: { DIALOG_ID: data.PARAMS.DIALOG_ID, MESSAGE: message },
                    requestId: 'reply',
                })
            }
            // ONIMBOTDELETE — configuration cleanup if necessary
            res.send('ok')
        } catch (error) {
            res.status(200).send('error: ' + error.message)
        }
    })

    app.listen(3000)
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Symfony\Component\HttpFoundation\Request;

    $request = Request::createFromGlobals();
    $event = (string)$request->request->get('event');
    $data = $request->request->all('data');
    $handlerUrl = 'https://your-domain.example/handler';

    $b24 = makeServiceBuilder($request); // see "SDK Initialization by Event Data"

    // imbot.* is not among the typed PHP services — called via the core
    if ($event === 'ONAPPINSTALL') {
        $b24->core->call('imbot.register', [
            'CODE' => 'ReportBot',
            'TYPE' => 'B',
            'EVENT_MESSAGE_ADD' => $handlerUrl,
            'EVENT_WELCOME_MESSAGE' => $handlerUrl,
            'EVENT_BOT_DELETE' => $handlerUrl,
            'PROPERTIES' => ['NAME' => 'Berichtender', 'COLOR' => 'AQUA', 'WORK_POSITION' => 'Reporting on affairs'],
        ]);
    } elseif ($event === 'ONIMBOTJOINCHAT') {
        $b24->core->call('imbot.message.add', [
            'DIALOG_ID' => $data['PARAMS']['DIALOG_ID'],
            'MESSAGE' => 'Hello! I am Berichtender. Ask [send=what\'s burning]What\'s burning?[/send]',
        ]);
    } elseif ($event === 'ONIMBOTMESSAGEADD') {
        $text = mb_strtolower($data['PARAMS']['MESSAGE'] ?? '');
        $userId = (int)$data['PARAMS']['FROM_USER_ID'];
        $message = 'I don\'t understand what you want to know.';
        if ($text === 'what\'s burning') {
            // result has the form {"tasks": [...]}, task fields are in lowercase (id, title, deadline)
            $result = $b24->core->call('tasks.task.list', [
                'filter' => ['RESPONSIBLE_ID' => $userId, '<DEADLINE' => date('c')],
                'select' => ['ID', 'TITLE', 'DEADLINE'],
            ])->getResponseData()->getResult();
            $tasks = $result['tasks'] ?? [];
            $message = $tasks
                ? "Overdue tasks:\n" . implode("\n", array_map(static fn($t) => '• ' . $t['title'], $tasks))
                : 'You\'re working brilliantly! Not a single overdue task.';
        }
        $b24->core->call('imbot.message.add', [
            'DIALOG_ID' => $data['PARAMS']['DIALOG_ID'],
            'MESSAGE' => $message,
        ]);
    }
    // ONIMBOTDELETE — configuration cleanup if necessary
    ```

- Python

    ```python
    # pip install b24pysdk flask
    from flask import Flask, request
    from datetime import datetime

    app = Flask(__name__)
    HANDLER_URL = "https://your-domain.example/handler"

    @app.post("/handler")
    def handler():
        event = request.form.get("event")
        auth = {k[5:-1]: v for k, v in request.form.items() if k.startswith("auth[")}
        data = request.form  # PARAMS arrive as data[PARAMS][...]
        client, token = make_client(auth)

        if event == "ONAPPINSTALL":
            # imbot.register is typed in b24pysdk
            client.imbot.register(
                code="ReportBot",
                properties={"NAME": "Berichtender", "COLOR": "AQUA", "WORK_POSITION": "Reporting on affairs"},
                event_message_add=HANDLER_URL,
                event_welcome_message=HANDLER_URL,
                event_bot_delete=HANDLER_URL,
                type="B",
            ).response
        elif event == "ONIMBOTJOINCHAT":
            client.imbot.message.add(
                "Hello! I am Berichtender. Ask [send=what's burning]What's burning?[/send]",
                dialog_id=data.get("data[PARAMS][DIALOG_ID]"),
            ).response
        elif event == "ONIMBOTMESSAGEADD":
            text = (data.get("data[PARAMS][MESSAGE]") or "").lower()
            user_id = int(data.get("data[PARAMS][FROM_USER_ID]") or 0)
            message = "I don't understand what you want to know."
            if text == "what's burning":
                tasks = client.tasks.task.list(
                    filter={"RESPONSIBLE_ID": user_id, "<DEADLINE": datetime.now().isoformat()},
                    select=["ID", "TITLE", "DEADLINE"],
                ).response.result["tasks"]
                message = (
                    "Overdue tasks:\n" + "\n".join(f"• {t['title']}" for t in tasks)
                    if tasks else "You're working brilliantly! Not a single overdue task."
                )
            client.imbot.message.add(
                message,
                dialog_id=data.get("data[PARAMS][DIALOG_ID]"),
            ).response
        # ONIMBOTDELETE — configuration cleanup if necessary
        return "ok"
    ```

{% endlist %}

After installing the application, the "Reporter" bot will appear in the General chat. Open a conversation with it and send "What's urgent?" to receive a list of overdue tasks.

## Running as a Local Application

1. Host the handler on a public HTTPS URL.
2. In the **Applications → Developer resources → Other → Local application** section, create a server-side application.
3. Enable Uses only API, and set both the handler path and the installation path to the same URL.
4. Grant permissions: `im`, `imbot`, `task`, `task_extended`.
5. Save and reinstall the application — the `ONAPPINSTALL` event will be sent, and the bot will register.
