# How to Create an Open Channels Connector for Website Chat

> Scope: [`imopenlines`](../../api-reference/scopes/permissions.md)
>
> Who can execute the scenario: a Bitrix24 administrator — the example checks permissions when the application is opened

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The connector transmits website visitor messages to Bitrix24 Open Channels and operator replies back to the website. The example transmits only message text: files and attachments require separate handling on the server and in the widget. The operator reply text arrives with BB codes, such as `[b]...[/b]` and `[br]`, and the sample widget displays them as is.

{% note warning "" %}

The connector works only within the context of an application (OAuth). An inbound webhook will not work: the `imconnector.*` methods require application authorization.

SDKs perform outgoing method calls. Your web server receives incoming events (`ONIMCONNECTORMESSAGEADD`) and connector settings (placement `SETTING_CONNECTOR`).

{% endnote %}

## Preparation

For the scenario, you need:

- a local application of the *Server* type with the `imopenlines` permission
- an active Open Channel to connect the channel to
- a public HTTPS URL for the application server side
- URL of the registration page `install_connector.*`
- URL of the event and settings handler `handler.*`
- HTML page of the chat widget `index.*`

In the examples, replace:

- `example_site_chat` with your connector code
- `https://your-domain.example/handler` with the event and settings handler URL: the `/handler` route for JS and Python or the `handler.php` file address for PHP. Specify the same address in the application settings
- `WIDGET_URI` with the HTTPS address of the chat widget, `WIDGET_NAME` with the channel name, and `CHAT_STORE` with the absolute path to the JSON storage file outside the public directory. Set them as environment variables on the server

Use a lowercase connector code: during registration, Bitrix24 converts `ID` to lowercase. Get the application ID and secret in its settings and set the `B24_CLIENT_ID` and `B24_CLIENT_SECRET` environment variables on the server. In `B24_DOMAIN`, set the known address of your Bitrix24 without the scheme and path, for example `example.bitrix24.com`. The secret and tokens stay on the server and do not get into the repository or the widget code.

In the application settings, specify `handler.*` in the *Initial installation path* field and `install_connector.*` in the *Your handler path* field: for JS and Python, these are the `/handler` and `/install_connector` routes, for PHP, the `handler.php` and `install_connector.php` file addresses. Leave the *Application completes the installation itself* option disabled: when the application is saved, Bitrix24 sends `ONAPPINSTALL` to `handler.*`.

1. Register the connector using the [imconnector.register](../../api-reference/imopenlines/imconnector/imconnector-register.md) method and subscribe to operator replies via [event.bind](../../api-reference/events/event-bind.md) — step 1
2. Connect the line via [imconnector.activate](../../api-reference/imopenlines/imconnector/imconnector-activate.md) and set the channel data via [imconnector.connector.data.set](../../api-reference/imopenlines/imconnector/imconnector-connector-data-set.md) — step 2
3. Pass the visitor message via [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md) — step 3
4. Receive the operator reply from the [OnImConnectorMessageAdd](../../api-reference/imopenlines/imconnector/events/on-im-connector-message-add.md) event and confirm delivery via [imconnector.send.status.delivery](../../api-reference/imopenlines/imconnector/imconnector-send-status-delivery.md) — the event handling function in step 2
5. Show the history and the send form in the widget — step 4

## Architecture and Storage

| File | Purpose |
|---|---|
| `function.*` | Storage and SDK initialization from the blocks below |
| `install_connector.*` | Registration and subscription, step 1 |
| `handler.*` | Installation, placement, and operator event, step 2 |
| `ajax.*` | Sending and history, step 3 |
| `index.*` | Widget, step 4 |

Choose one language. In PHP, save the storage and SDK initialization blocks, as well as the `handlePlacement` and `handleEvent` functions from step 2, in `function.php`; the other files include it via `require_once`. For JS, assemble the server fragments in order in `app.mjs`, for Python, in `app.py`; the file names in the table denote their roles.

The example uses one Bitrix24 and one line per connector. For multiple lines, store the "chat → line" mapping. Place the JSON file outside the public directory with permissions for the application user only; for production, use a database with transactions. The file example is designed for a single process without concurrent writes. `saveAuth` retains authorization by `member_id`; the selected line determines whose tokens to load for AJAX.

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    import fs from 'node:fs'
    import { randomUUID } from 'node:crypto'
    const file = process.env.CHAT_STORE // absolute path outside the public directory
    const read = () => fs.existsSync(file) ? JSON.parse(fs.readFileSync(file, 'utf8')) : { auth: {}, chats: {} }
    const write = data => fs.writeFileSync(file, JSON.stringify(data), { mode: 0o600 })
    const getConnectorID = () => 'example_site_chat'
    function setLine(line, member) {
        const s = read();
        s.line = line;
        s.member = member;
        write(s)
    }
    const getLine = () => read().line
    function saveAuth(auth) {
        const s = read();
        s.auth[auth.member_id] = { ...s.auth[auth.member_id], ...auth };
        write(s)
    }
    const loadAuth = (member = read().member) => read().auth[member]
    function saveMessage(chat, message) {
        const s = read(), id = String(message.message.id ?? (message.im ? `operator-${message.im.message_id}` : randomUUID()))
        const rows = s.chats[chat] ??= []
        if (!rows.some(row => row.id === id)) rows.push({ id, name: message.user?.name ?? 'Operator', text: message.message.text })
        write(s); return id
    }
    const getChat = chat => read().chats[chat] ?? []
    ```

- Python

    ```python
    import json, os, uuid
    from pathlib import Path
    FILE = Path(os.environ["CHAT_STORE"])  # outside the public directory
    def read():
        return json.loads(FILE.read_text()) if FILE.exists() else {"auth": {}, "chats": {}}
    def write(data):
        with os.fdopen(os.open(FILE, os.O_WRONLY | os.O_CREAT | os.O_TRUNC, 0o600), "w") as out:
            json.dump(data, out)
    def get_connector_id(): return "example_site_chat"
    def set_line(line, member):
        s = read()
        s.update(line=line, member=member)
        write(s)
    def get_line(): return read()["line"]
    def save_auth(auth):
        s = read(); key = auth["member_id"]
        s["auth"][key] = {**s["auth"].get(key, {}), **auth}; write(s)
    def load_auth(member=None): return read()["auth"].get(member or read().get("member"))
    def save_message(chat, message):
        s = read(); mid = str(message["message"].get("id") or (f"operator-{message['im']['message_id']}" if "im" in message else uuid.uuid4()))
        rows = s["chats"].setdefault(str(chat), [])
        if not any(row["id"] == mid for row in rows):
            rows.append({"id": mid, "name": message.get("user", {}).get("name", "Operator"), "text": message["message"]["text"]})
        write(s); return mid
    def get_chat(chat): return read()["chats"].get(str(chat), [])
    ```

- PHP

    ```php
    <?php
    function readStore() { return is_file(getenv('CHAT_STORE')) ? json_decode(file_get_contents(getenv('CHAT_STORE')), true) : ['auth' => [], 'chats' => []]; }
    function writeStore($s) {
        $mask = umask(0077);
        file_put_contents(getenv('CHAT_STORE'), json_encode($s), LOCK_EX);
        umask($mask);
    }
    function getConnectorID() { return 'example_site_chat'; }
    function setLine($line, $member) {
        $s = readStore();
        $s['line'] = $line;
        $s['member'] = $member;
        writeStore($s);
    }
    function getLine() { return readStore()['line']; }
    function saveAuth($auth) {
        $s = readStore();
        $key = $auth['member_id'];
        $s['auth'][$key] = array_merge($s['auth'][$key] ?? [], $auth);
        writeStore($s);
    }
    function loadAuth($member = null) { $s = readStore(); return $s['auth'][$member ?? $s['member'] ?? ''] ?? null; }
    function saveMessage($chat, $message) {
        $s = readStore(); $id = (string)($message['message']['id'] ?? (isset($message['im']) ? 'operator-' . $message['im']['message_id'] : bin2hex(random_bytes(16))));
        $rows = $s['chats'][$chat] ?? [];
        if (!in_array($id, array_column($rows, 'id'), true)) {
            $rows[] = ['id' => $id, 'name' => $message['user']['name'] ?? 'Operator', 'text' => $message['message']['text']];
        }
        $s['chats'][$chat] = $rows;
        writeStore($s);
        return $id;
    }
    function getChat($chat) { return readStore()['chats'][$chat] ?? []; }
    ```

{% endlist %}

## Initialize the SDK

Events arrive as a POST request `application/x-www-form-urlencoded` with keys `auth[access_token]`, `data[MESSAGES][0][im][chat_id]`. Express parses them into a nested object, PHP into `$_POST`; Flask needs `unflatten`, which turns numeric message keys into a dictionary.

For [placement and the installation page](../../api-reference/widgets/index.md), authorization arrives in separate fields `AUTH_ID`, `REFRESH_ID`, `member_id` and the `DOMAIN` parameter in the query string, so a separate constructor is used for them. The widget AJAX does not carry Bitrix24 authorization: the client is created from the retained tokens. Tokens renewed by the SDK are retained as well.

{% list tabs %}

- JS

    ```js
    // npm install express @bitrix24/b24jssdk@3.0.0
    import express from 'express'
    import { fileURLToPath } from 'node:url'
    import { B24OAuth } from '@bitrix24/b24jssdk'
    const app = express()
    app.use(express.urlencoded({ extended: true }))
    const visitorChat = req => req.headers.cookie?.match(/(?:^|;\s*)chat_id=([^;]+)(?:;|$)/)?.[1]
    app.get('/', (req, res) => {
        if (!visitorChat(req)) res.cookie('chat_id', randomUUID(), { httpOnly: true, secure: true, sameSite: 'lax', path: '/' })
        res.sendFile(fileURLToPath(new URL('./index.html', import.meta.url)))
    })
    const widgetUri = process.env.WIDGET_URI, widgetName = process.env.WIDGET_NAME
    const APP = { clientId: process.env.B24_CLIENT_ID, clientSecret: process.env.B24_CLIENT_SECRET }
    function fromEvent(auth) {
        if (auth.domain !== process.env.B24_DOMAIN) throw new Error('Unknown domain')
        const params = {
            domain: auth.domain, accessToken: auth.access_token, refreshToken: auth.refresh_token,
            memberId: auth.member_id, applicationToken: auth.application_token, userId: Number(auth.user_id ?? 0),
            expires: Number(auth.expires ?? Date.now() / 1000 + Number(auth.expires_in)), expiresIn: Number(auth.expires_in),
            scope: auth.scope, status: auth.status, clientEndpoint: `https://${process.env.B24_DOMAIN}/rest/`, serverEndpoint: 'https://oauth.bitrix.info/rest/',
        }
        const b24 = new B24OAuth(params, APP)
        b24.offClientSideWarning()
        b24.setCallbackRefreshAuth(async ({ b24OAuthParams: p }) => {
            saveAuth({ ...auth, access_token: p.accessToken, refresh_token: p.refreshToken, expires: p.expires, expires_in: p.expiresIn })
        })
        return b24
    }
    function placementAuth(req) {
        const p = req.body
        return { domain: req.query.DOMAIN, access_token: p.AUTH_ID, refresh_token: p.REFRESH_ID,
            expires: Math.floor(Date.now() / 1000) + Number(p.AUTH_EXPIRES), expires_in: Number(p.AUTH_EXPIRES), member_id: p.member_id, application_token: p.APPLICATION_TOKEN,
            scope: p.APPLICATION_SCOPE, status: p.status, server_endpoint: p.SERVER_ENDPOINT,
            client_endpoint: `https://${req.query.DOMAIN}/rest/` }
    }
    const fromPlacement = req => fromEvent(placementAuth(req))
    const fromStore = () => fromEvent(loadAuth())
    ```

- Python

    ```python
    # pip install b24pysdk flask
    import re, time
    from flask import Flask, request, jsonify, abort, g, send_from_directory
    from b24pysdk import BitrixApp, BitrixToken, Client
    app = Flask(__name__)
    @app.get("/")
    def widget():
        response = send_from_directory(app.root_path, "index.html")
        if not request.cookies.get("chat_id"):
            response.set_cookie("chat_id", str(uuid.uuid4()), httponly=True, secure=True, samesite="Lax", path="/")
        return response
    widget_uri = os.environ["WIDGET_URI"]; widget_name = os.environ["WIDGET_NAME"]
    APP = BitrixApp(client_id=os.environ["B24_CLIENT_ID"], client_secret=os.environ["B24_CLIENT_SECRET"])
    def unflatten(form):
        result = {}
        for key, value in form.items():
            path = re.findall(r"[^\[\]]+", key); node = result
            for part in path[:-1]: node = node.setdefault(part, {})
            node[path[-1]] = value
        return result
    def from_event(auth):
        if auth["domain"] != os.environ["B24_DOMAIN"]: abort(403)
        token = BitrixToken(domain=auth["domain"], auth_token=auth["access_token"],
                            refresh_token=auth["refresh_token"], bitrix_app=APP)
        g.credentials = (auth, token)
        return Client(token), token
    def placement_auth():
        p = request.form
        return dict(domain=request.args["DOMAIN"], access_token=p["AUTH_ID"], refresh_token=p["REFRESH_ID"],
                    expires_in=int(p["AUTH_EXPIRES"]), member_id=p["member_id"], application_token=p["APPLICATION_TOKEN"])
    def from_placement(): return from_event(placement_auth())
    def from_store(): return from_event(load_auth())
    @app.after_request
    def persist_tokens(response):
        if hasattr(g, "credentials"):
            auth, token = g.credentials; saved = load_auth(auth.get("member_id", ""))
            if saved and saved.get("application_token") == auth.get("application_token"):
                save_auth({**auth, "access_token": token.auth_token, "refresh_token": token.refresh_token})
        return response
    ```

- PHP

    ```php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';
    use Bitrix24\SDK\Core\Credentials\{ApplicationProfile, AuthToken, DefaultOAuthServerUrl};
    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Bitrix24\SDK\Events\AuthTokenRenewedEvent;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    function fromEvent($auth) {
        if ($auth['domain'] !== getenv('B24_DOMAIN')) { http_response_code(403); exit; }
        $profile = ApplicationProfile::initFromArray([
            'BITRIX24_PHP_SDK_APPLICATION_CLIENT_ID' => getenv('B24_CLIENT_ID'),
            'BITRIX24_PHP_SDK_APPLICATION_CLIENT_SECRET' => getenv('B24_CLIENT_SECRET'),
            'BITRIX24_PHP_SDK_APPLICATION_SCOPE' => 'imopenlines',
        ]);
        $token = new AuthToken($auth['access_token'], $auth['refresh_token'], (int)($auth['expires'] ?? time() + (int)$auth['expires_in']));
        $dispatcher = new EventDispatcher();
        $dispatcher->addListener(AuthTokenRenewedEvent::class, function ($event) use ($auth) {
            $t = $event->getRenewedToken()->authToken;
            saveAuth(array_merge($auth, ['access_token' => $t->accessToken, 'refresh_token' => $t->refreshToken, 'expires' => $t->expires]));
        });
        return (new ServiceBuilderFactory($dispatcher, new Logger('chat')))
            ->init($profile, $token, $auth['domain'], DefaultOAuthServerUrl::default());
    }
    function placementAuth() {
        return ['domain' => $_GET['DOMAIN'], 'access_token' => $_POST['AUTH_ID'], 'refresh_token' => $_POST['REFRESH_ID'],
            'expires' => time() + (int)$_POST['AUTH_EXPIRES'], 'expires_in' => (int)$_POST['AUTH_EXPIRES'], 'member_id' => $_POST['member_id'], 'application_token' => $_POST['APPLICATION_TOKEN']];
    }
    function fromPlacement() { return fromEvent(placementAuth()); }
    function fromStore() { return fromEvent(loadAuth()); }
    ```

{% endlist %}

## 1. Installation: Register a Connector

When the application is opened, register the connector using the [imconnector.register](../../api-reference/imopenlines/imconnector/imconnector-register.md) method and subscribe to the [OnImConnectorMessageAdd](../../api-reference/imopenlines/imconnector/events/on-im-connector-message-add.md) event using the [event.bind](../../api-reference/events/event-bind.md) method.

The installation page receives a placement request. The [app.info](../../api-reference/common/system/app-info.md) call verifies OAuth before the credentials are retained: `CODE` must match `B24_CLIENT_ID`. The [user.admin](../../api-reference/common/users/user-admin.md) call lets only a Bitrix24 administrator proceed. The constructors accept only the preset `B24_DOMAIN`. Before subscribing, [event.get](../../api-reference/events/event-get.md) checks whether a handler already exists, so reopening the application does not create the subscription again. `event.get` returns the event name in uppercase, so the handler is looked up by `ONIMCONNECTORMESSAGEADD`. The connector icon is `ICON.DATA_IMAGE`; replace it with your channel's image.

In `imconnector.register`, pass: `ID` — the connector identifier, `NAME` — the name, `ICON`/`ICON_DISABLED` — icons (DATA representation of SVG), `PLACEMENT_HANDLER` — the settings handler URL.

{% list tabs %}

- JS

    ```js
    app.post('/install_connector', async (req, res) => {
        const $b24 = fromPlacement(req)
        const info = await $b24.actions.v2.call.make({ method: 'app.info', requestId: 'verify-install' })
        if (info.getData()?.result?.CODE !== APP.clientId) return res.sendStatus(403)
        const admin = await $b24.actions.v2.call.make({ method: 'user.admin', requestId: 'check-admin' })
        if (admin.getData()?.result !== true) return res.sendStatus(403)
        saveAuth(placementAuth(req)) // tokens were just issued, the SDK did not renew them during the checks
        const connectorId = getConnectorID()
        const handlerUrl = 'https://your-domain.example/handler'
        const icon = {
            DATA_IMAGE: 'data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20viewBox%3D%220%200%2070%2070%22%3E%3Ccircle%20cx%3D%2235%22%20cy%3D%2235%22%20r%3D%2228%22%20fill%3D%22%23ffffff%22/%3E%3C/svg%3E',
            COLOR: '#a6ffa3', SIZE: '100%', POSITION: 'center',
        }
        const reg = await $b24.actions.v2.call.make({
            method: 'imconnector.register',
            params: { ID: connectorId, NAME: 'ExampleSiteChat', ICON: icon, ICON_DISABLED: { ...icon, COLOR: '#ffb3a3' }, PLACEMENT_HANDLER: handlerUrl },
            requestId: 'connector-register',
        })
        if (reg.getData().result.result !== true) return res.sendStatus(502)
        const handlers = await $b24.actions.v2.call.make({ method: 'event.get', requestId: 'event-get' })
        const bound = handlers.getData().result.some(h => h.event === 'ONIMCONNECTORMESSAGEADD' && h.handler === handlerUrl)
        if (!bound) {
            const binding = await $b24.actions.v2.call.make({
                method: 'event.bind',
                params: { event: 'OnImConnectorMessageAdd', handler: handlerUrl },
                requestId: 'event-bind',
            })
            if (binding.getData()?.result !== true) return res.sendStatus(502)
        }
        res.send('Connector registered')
    })
    ```

- Python

    ```python
    @app.post("/install_connector")
    def install_connector():
        client, token = from_placement()
        if token.call_method("app.info", {}).get("result", {}).get("CODE") != os.environ["B24_CLIENT_ID"]: abort(403)
        if token.call_method("user.admin", {}).get("result") is not True: abort(403)
        save_auth(placement_auth())  # tokens were just issued, the SDK did not renew them during the checks
        connector_id = get_connector_id()
        handler_url = "https://your-domain.example/handler"
        icon = {
            "DATA_IMAGE": "data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20viewBox%3D%220%200%2070%2070%22%3E%3Ccircle%20cx%3D%2235%22%20cy%3D%2235%22%20r%3D%2228%22%20fill%3D%22%23ffffff%22/%3E%3C/svg%3E",
            "COLOR": "#a6ffa3", "SIZE": "100%", "POSITION": "center",
        }
        reg = client.imconnector.register(
            bitrix_id=connector_id,
            name="ExampleSiteChat",
            icon=icon,
            placement_handler=handler_url,
            icon_disabled={**icon, "COLOR": "#ffb3a3"},
        ).response
        if reg.result["result"] is not True: abort(502)
        handlers = token.call_method("event.get", {}).get("result", [])
        if not any(h["event"] == "ONIMCONNECTORMESSAGEADD" and h["handler"] == handler_url for h in handlers):
            binding = client.event.bind(event="OnImConnectorMessageAdd", handler=handler_url).response
            if binding.result is not True: abort(502)
        return "Connector registered", 200
    ```


- PHP

    ```php
    <?php
    require_once 'function.php';
    $b24 = fromPlacement();
    $info = $b24->core->call('app.info', [])->getResponseData()->getResult();
    if (($info['CODE'] ?? '') !== getenv('B24_CLIENT_ID')) { http_response_code(403); exit; }
    // the SDK returns a scalar result as element [0]
    $isAdmin = $b24->core->call('user.admin', [])->getResponseData()->getResult()[0] ?? false;
    if ($isAdmin !== true) { http_response_code(403); exit; }
    saveAuth(placementAuth()); // tokens were just issued, the SDK did not renew them during the checks
    $connectorId = getConnectorID();
    $handlerUrl = 'https://your-domain.example/handler';
    $icon = [
        'DATA_IMAGE' => 'data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20viewBox%3D%220%200%2070%2070%22%3E%3Ccircle%20cx%3D%2235%22%20cy%3D%2235%22%20r%3D%2228%22%20fill%3D%22%23ffffff%22/%3E%3C/svg%3E',
        'COLOR' => '#a6ffa3', 'SIZE' => '100%', 'POSITION' => 'center',
    ];
    $reg = $b24->getIMOpenLinesScope()->connector()->register([
        'ID' => $connectorId,
        'NAME' => 'ExampleSiteChat',
        'ICON' => $icon,
        'ICON_DISABLED' => array_merge($icon, ['COLOR' => '#ffb3a3']),
        'PLACEMENT_HANDLER' => $handlerUrl,
    ]);
    // RegisterResult::isSuccess() checks the inner registration result
    if (!$reg->isSuccess()) { http_response_code(502); exit; }
    $handlers = $b24->core->call('event.get', [])->getResponseData()->getResult();
    $bound = false;
    foreach ($handlers as $h) {
        $bound = $bound || ($h['event'] === 'ONIMCONNECTORMESSAGEADD' && $h['handler'] === $handlerUrl);
    }
    if (!$bound) {
        // event.bind is not part of the typed connector service, so call it via the core
        $binding = $b24->core->call('event.bind', ['event' => 'OnImConnectorMessageAdd', 'handler' => $handlerUrl]);
        if (($binding->getResponseData()->getResult()[0] ?? false) !== true) { http_response_code(502); exit; }
    }
    echo 'Connector registered';
    ```

{% endlist %}

The shortened registration response is `result.result: true`. The outer `result` is the REST wrapper, the inner one is the registration result:

```json
{ "result": { "result": true } }
```

The `event.bind` subscription returns `{"result": true}` separately. If the inner registration `result` is negative, return HTTP 502 and do not call `event.bind`.

## 2. Handler: Installation, Activation, and Operator Replies

The `POST /handler` route for JS and Python and the `handler.php` file for PHP first distinguish `event` from `PLACEMENT`. The `ONAPPINSTALL` event arrives first — when the application is saved, before the installation page from step 1 is opened. On `ONAPPINSTALL`, create a client from `auth` using the event constructor, call `app.info` to verify OAuth, then retain `auth` via `saveAuth` / `save_auth` and return HTTP 200. The retained `auth.application_token` is used to [verify subsequent events](../../api-reference/events/safe-event-handlers.md). Do not overwrite it with data from an unverified event.

{% list tabs %}

- JS

    ```js
    app.post('/handler', async (req, res) => {
        try {
            if (req.body.event === 'ONAPPINSTALL') {
                const auth = req.body.auth, b24 = fromEvent(auth)
                const info = await b24.actions.v2.call.make({ method: 'app.info', requestId: 'verify-event' })
                if (info.getData()?.result?.CODE !== APP.clientId) return res.sendStatus(403)
                saveAuth(auth); return res.sendStatus(200)
            }
            if (req.body.PLACEMENT === 'SETTING_CONNECTOR') return await handlePlacement(req, res)
            return await handleEvent(req, res)
        } catch { return res.sendStatus(502) }
    })
    ```

- Python

    ```python
    @app.post("/handler")
    def handler():
        if request.form.get("event") == "ONAPPINSTALL":
            auth = unflatten(request.form)["auth"]; client, token = from_event(auth)
            if token.call_method("app.info", {}).get("result", {}).get("CODE") != os.environ["B24_CLIENT_ID"]: abort(403)
            save_auth(auth)
            return "", 200
        if request.form.get("PLACEMENT") == "SETTING_CONNECTOR": return handle_placement()
        return handle_event()
    ```

- PHP

    ```php
    <?php
    require_once 'function.php';
    if (($_POST['event'] ?? '') === 'ONAPPINSTALL') {
        $auth = $_POST['auth']; $b24 = fromEvent($auth);
        $info = $b24->core->call('app.info', [])->getResponseData()->getResult();
        if (($info['CODE'] ?? '') !== getenv('B24_CLIENT_ID')) { http_response_code(403); exit; }
        saveAuth($auth);
    } elseif (($_POST['PLACEMENT'] ?? '') === 'SETTING_CONNECTOR') {
        handlePlacement();
    } else { handleEvent(); }
    ```

{% endlist %}

**Activation.** Bitrix24 opens the `SETTING_CONNECTOR` placement after **Connect** is clicked. `ACTIVE_STATUS` in `PLACEMENT_OPTIONS` shows the current state; to connect, pass an explicit `ACTIVE: 1`, to disconnect, `0`. Then `imconnector.connector.data.set` sets the external channel `ID`, the `URL_IM` link, and the `NAME`. Retain the placement authorization before the calls so that tokens renewed by the SDK are not overwritten by old ones. After both calls succeed, retain `LINE` and `member_id`.

Settings function:

{% list tabs %}

- JS

    ```js
    async function handlePlacement(req, res) {
        const auth = placementAuth(req), saved = loadAuth(auth.member_id)
        if (!saved?.application_token || saved.application_token !== auth.application_token) return res.sendStatus(403)
        saveAuth(auth) // before the calls: tokens renewed by the SDK will overwrite this entry
        const $b24 = fromPlacement(req), connectorId = getConnectorID()
        const line = Number(JSON.parse(req.body.PLACEMENT_OPTIONS).LINE)
        const active = await $b24.actions.v2.call.make({
            method: 'imconnector.activate', params: { CONNECTOR: connectorId, LINE: line, ACTIVE: 1 }, requestId: 'activate',
        })
        if (!active.getData().result) throw new Error('Activation failed')
        const channel = await $b24.actions.v2.call.make({ method: 'imconnector.connector.data.set', requestId: 'data',
            params: { CONNECTOR: connectorId, LINE: line, DATA: { ID: `${connectorId}_${line}`, URL_IM: widgetUri, NAME: widgetName } } })
        if (!channel.getData().result) throw new Error('Channel data not saved')
        setLine(line, auth.member_id)
        res.send('Channel connected')
    }
    ```

- Python

    ```python
    def handle_placement():
        auth = placement_auth(); saved = load_auth(auth["member_id"])
        if not saved or not saved.get("application_token") or saved["application_token"] != auth["application_token"]: abort(403)
        save_auth(auth)  # before the calls: tokens renewed by the SDK will overwrite this entry
        client, token = from_placement(); connector_id = get_connector_id()
        line = int(json.loads(request.form["PLACEMENT_OPTIONS"])["LINE"])
        active = client.imconnector.activate(connector=connector_id, line=line, active=1).response
        if not active.result: abort(502)
        channel = client.imconnector.connector.data.set(connector=connector_id, line=line,
            data={"ID": f"{connector_id}_{line}", "URL_IM": widget_uri, "NAME": widget_name}).response
        if not channel.result: abort(502)
        set_line(line, auth["member_id"])
        return "Channel connected", 200
    ```

- PHP

    ```php
    function handlePlacement() {
        $widgetUri = getenv('WIDGET_URI'); $widgetName = getenv('WIDGET_NAME');
        $auth = placementAuth(); $saved = loadAuth($auth['member_id']);
        if (empty($saved['application_token']) || $saved['application_token'] !== $auth['application_token']) { http_response_code(403); exit; }
        saveAuth($auth); // before the calls: tokens renewed by the SDK will overwrite this entry
        $b24 = fromPlacement(); $connectorId = getConnectorID();
        $line = (string)(int)json_decode($_POST['PLACEMENT_OPTIONS'], true)['LINE'];
        $active = $b24->getIMOpenLinesScope()->connector()->activate($connectorId, $line, 1);
        if (!$active->isSuccess()) { http_response_code(502); exit; }
        $channel = $b24->getIMOpenLinesScope()->connector()->setData($connectorId, $line, [
            'ID' => $connectorId . '_' . $line, 'URL_IM' => $widgetUri, 'NAME' => $widgetName,
        ]);
        if (!$channel->isSuccess()) { http_response_code(502); exit; }
        setLine($line, $auth['member_id']);
        echo 'Channel connected';
    }
    ```

{% endlist %}

Successful `imconnector.activate` and `imconnector.connector.data.set` return `{"result": true}`.

**Operator reply.** Before changing the history, verify the token and the connector code. From `data.MESSAGES`, take the external `chat.id`, the original text, and the internal `im` object. Pass the external identifier returned by `saveMessage` as an array in `message.id` of the delivery confirmation.

{% list tabs %}

- JS

    ```js
    async function handleEvent(req, res) {
        const { event, auth = {}, data = {} } = req.body
        const saved = loadAuth(auth.member_id ?? '')
        if (!saved?.application_token || saved.application_token !== auth.application_token) return res.sendStatus(403)
        if (event !== 'ONIMCONNECTORMESSAGEADD' || data.CONNECTOR !== getConnectorID()) return res.sendStatus(200)
        const $b24 = fromEvent(saved)
        for (const message of Object.values(data.MESSAGES ?? {})) {
            const id = saveMessage(message.chat.id, message)
            const delivery = await $b24.actions.v2.call.make({ method: 'imconnector.send.status.delivery', requestId: `delivery-${id}`,
                params: { CONNECTOR: getConnectorID(), LINE: getLine(), MESSAGES: [{
                    im: message.im, chat: message.chat, message: { id: [id], date: Math.floor(Date.now() / 1000) },
                }] } })
            if (!delivery.getData().result?.SUCCESS) return res.sendStatus(502)
        }
        res.sendStatus(200)
    }
    ```

- Python

    ```python
    def handle_event():
        payload = unflatten(request.form)
        auth = payload.get("auth", {}); data = payload.get("data", {})
        saved = load_auth(auth.get("member_id", ""))
        if not saved or not saved.get("application_token") or saved["application_token"] != auth.get("application_token"): abort(403)
        if payload.get("event") == "ONIMCONNECTORMESSAGEADD" and data.get("CONNECTOR") == get_connector_id():
            client, token = from_event(saved)
            messages = data.get("MESSAGES", {}).values()
            for message in messages:
                mid = save_message(message["chat"]["id"], message)
                delivery = client.imconnector.send.status.delivery(connector=get_connector_id(), line=get_line(), messages=[{
                    "im": message["im"], "chat": message["chat"], "message": {"id": [mid], "date": int(time.time())},
                }]).response
                if not delivery.result["SUCCESS"]: abort(502)
        return "", 200
    ```

- PHP

    ```php
    function handleEvent() {
        $auth = $_POST['auth'] ?? []; $data = $_POST['data'] ?? [];
        $saved = loadAuth($auth['member_id'] ?? '');
        if (empty($saved['application_token']) || $saved['application_token'] !== ($auth['application_token'] ?? '')) { http_response_code(403); exit; }
        if (($_POST['event'] ?? '') === 'ONIMCONNECTORMESSAGEADD' && ($data['CONNECTOR'] ?? '') === getConnectorID()) {
            $b24 = fromEvent($saved);
            foreach ($data['MESSAGES'] as $message) {
                $id = saveMessage($message['chat']['id'], $message);
                $delivery = $b24->getIMOpenLinesScope()->connector()->sendStatusDelivery(getConnectorID(), (string)getLine(), [[
                    'im' => $message['im'], 'chat' => $message['chat'], 'message' => ['id' => [$id], 'date' => time()],
                ]]);
                if (!$delivery->isSuccess()) { http_response_code(502); exit; }
            }
        }
        http_response_code(200);
    }
    ```

{% endlist %}

Successful delivery confirmation:

```json
{ "result": { "SUCCESS": true, "DATA": [] } }
```

## 3. AJAX: Sending and History

Host the AJAX and the widget on the same origin. GET `?action=history` returns the history, POST accepts `name` and `message`. The server takes the external `chat.id` only from the `chat_id` cookie set when the widget is opened; without it, it returns HTTP 403. Any request with this cookie value receives the history, so in a production service bind the cookie to the visitor session and rate-limit requests on the web server. Pass the original text to REST; use `textContent` for output.

First, build a message with the external identifiers `user.id`, `chat.id`, `message.id`, send `imconnector.send.messages`, check for success, and retain it for the widget. Handle SDK errors as HTTP 502 without exposing tokens to the client.

{% list tabs %}

- JS

    ```js
    app.all('/ajax', async (req, res) => {
        const chat = visitorChat(req)
        if (!chat) return res.sendStatus(403)
        if (req.method === 'GET' && req.query.action === 'history') return res.json(getChat(chat))
        if (req.method !== 'POST') return res.sendStatus(405)
        const text = String(req.body.message ?? '').trim()
        if (!text) return res.sendStatus(400)
        try {
            const message = { user: { id: chat, name: String(req.body.name || 'Visitor') },
                chat: { id: chat }, message: { id: randomUUID(), date: Math.floor(Date.now() / 1000), text } }
            const result = await fromStore().actions.v2.call.make({ method: 'imconnector.send.messages', requestId: message.message.id,
                params: { CONNECTOR: getConnectorID(), LINE: getLine(), MESSAGES: [message] } })
            const sent = result.getData().result
            if (!sent.SUCCESS || !sent.DATA.RESULT[0].SUCCESS) return res.sendStatus(502)
            saveMessage(chat, message)
            res.json(getChat(chat))
        } catch { res.sendStatus(502) }
    })
    app.listen(process.env.PORT || 3000)
    ```

- Python

    ```python
    @app.route("/ajax", methods=["GET", "POST"])
    def ajax():
        chat = request.cookies.get("chat_id", "")
        if not chat: abort(403)
        if request.method == "GET" and request.args.get("action") == "history": return jsonify(get_chat(chat))
        if request.method != "POST": abort(405)
        text = request.form.get("message", "").strip()
        if not text: abort(400)
        message = {"user": {"id": chat, "name": request.form.get("name") or "Visitor"},
                   "chat": {"id": chat}, "message": {"id": str(uuid.uuid4()), "date": int(time.time()), "text": text}}
        try:
            client, token = from_store()
            sent = client.imconnector.send.messages(connector=get_connector_id(), line=get_line(), messages=[message]).response.result
            if not sent["SUCCESS"] or not sent["DATA"]["RESULT"][0]["SUCCESS"]: abort(502)
            save_message(chat, message)
        except Exception:
            abort(502)
        return jsonify(get_chat(chat))
    ```

- PHP

    ```php
    <?php
    require_once 'function.php';
    header('Content-Type: application/json; charset=utf-8');
    $chat = (string)($_COOKIE['chat_id'] ?? '');
    if ($chat === '') { http_response_code(403); exit; }
    if ($_SERVER['REQUEST_METHOD'] === 'GET' && ($_GET['action'] ?? '') === 'history') { echo json_encode(getChat($chat)); exit; }
    if ($_SERVER['REQUEST_METHOD'] !== 'POST') { http_response_code(405); exit; }
    $text = trim((string)($_POST['message'] ?? ''));
    if ($text === '') { http_response_code(400); exit; }
    try {
        $message = ['user' => ['id' => $chat, 'name' => ($_POST['name'] ?? '') ?: 'Visitor'],
            'chat' => ['id' => $chat], 'message' => ['id' => bin2hex(random_bytes(16)), 'date' => time(), 'text' => $text]];
        $sent = fromStore()->getIMOpenLinesScope()->connector()->sendMessages(getConnectorID(), (string)getLine(), [$message]);
        if (!$sent->isSuccess() || !$sent->getData()['RESULT'][0]['SUCCESS']) { http_response_code(502); exit; }
        saveMessage($chat, $message);
        echo json_encode(getChat($chat));
    } catch (Throwable $error) { http_response_code(502); }
    ```

{% endlist %}

The shortened successful REST response of the send contains the internal Bitrix24 session and chat identifiers. They help with diagnostics and do not replace the external `chat.id`:

```json
{ "result": { "SUCCESS": true, "DATA": { "RESULT": [
    { "SUCCESS": true, "session": { "ID": "323", "CHAT_ID": "1767" } }
] } } }
```

## 4. Website Chat Widget

Save the HTML below to `index.html` next to the server file. JS and Python serve it with the `GET /` route from the initialization section. For PHP, replace `/ajax` with `/ajax.php` in the HTML and create `index.php` next to it:

```php
<?php
if (empty($_COOKIE['chat_id'])) {
    setcookie('chat_id', bin2hex(random_bytes(16)), ['path' => '/', 'httponly' => true, 'secure' => true, 'samesite' => 'Lax']);
}
header('Content-Type: text/html; charset=utf-8');
readfile(__DIR__ . '/index.html');
```

`WIDGET_URI` is the HTTPS address of the `GET /` route or `index.php`. On first opening, the server writes a random identifier to the `chat_id` cookie with the `HttpOnly`, `Secure`, `SameSite=Lax` flags.

```html
<!doctype html>
<html lang="en">
<meta charset="utf-8">
<title>Chat with an operator</title>
<body>
<ul id="history"></ul>
<form id="chat">
  <input name="name" placeholder="Name" required>
  <input name="message" placeholder="Message" required>
  <button>Send</button>
</form>
<p id="error" role="status"></p>
<script>
const endpoint = '/ajax', form = document.querySelector('#chat')
async function exchange(options) {
  const url = `${endpoint}?action=history`
  try {
    const response = await fetch(options ? endpoint : url, options)
    if (!response.ok) throw new Error('Failed to update the chat')
    const rows = await response.json()
    document.querySelector('#history').replaceChildren(...rows.map(row => {
      const li = document.createElement('li');
      li.textContent = `${row.name}: ${row.text}`;
      return li
    }))
    document.querySelector('#error').textContent = ''
  } catch (error) { document.querySelector('#error').textContent = error.message }
}
form.addEventListener('submit', async event => {
  event.preventDefault()
  const body = new URLSearchParams(new FormData(form))
  await exchange({ method: 'POST', body })
})
exchange(); setInterval(exchange, 3000)
</script>
</body>
</html>
```

## 5. Running the Connector

1. Set the server variables `B24_CLIENT_ID`, `B24_CLIENT_SECRET`, `B24_DOMAIN`, `CHAT_STORE`, `WIDGET_URI`, `WIDGET_NAME`
2. Deploy the server files and `index.html` to a public HTTPS URL: run JS with `node app.mjs`, Python with `flask --app app run` behind an HTTPS reverse proxy; for PHP, configure a web server with PHP
3. Save the application in Bitrix24 and wait for `ONAPPINSTALL`: `handler.*` retains the event verification token. Open the application — the `install_connector.*` page registers the connector and the subscription
4. In the **Contact Center**, open `ExampleSiteChat`, select an Open Channel, and click **Connect** — Bitrix24 calls `handler.*` with placement `SETTING_CONNECTOR`
5. Open `WIDGET_URI` and test the message exchange

## Check the Result

Send a message from the widget. A conversation should appear in the selected Open Channel. The operator's reply should appear on the website after the next poll. In the JSON storage, check the retained line and both history entries for the same external `chat.id`. For diagnostics, temporarily log `SUCCESS` and `session.CHAT_ID` from the send response, and `im.chat_id` and `im.message_id` from the event, to the server log. Do not write tokens to the log.

## Errors and Diagnostics

- The widget does not open — check the `GET /` route or `index.php`, `WIDGET_URI`, and HTTPS
- `WRONG_AUTH_TYPE` — the call was made outside an OAuth application context
- `ERROR_ARGUMENT` — check the required `CONNECTOR`, `LINE`, `MESSAGES`, `DATA`, `ACTIVE` against the page of the called method
- `ERROR_EVENT_NOT_FOUND` — fix the event name in `event.bind`
- `ERROR_CORE` with the text `Handler already binded` — a subscription to this address already exists, check it using the [event.get](../../api-reference/events/event-get.md) method
- HTTP 403 on the installation page — the application was opened by someone other than a Bitrix24 administrator, or `app.info` returned a different `CODE`
- `NOT_ACTIVE_LINE` — check that the selected line exists and is active
- The settings opened, but the channel did not connect — check `LINE` and the explicit `ACTIVE: 1`, repeat the activation in step 2
- HTTP 403 from the handler — compare `member_id` and the retained `application_token`; check that `ONAPPINSTALL` was received, do not disable token verification
- Empty `auth` or `MESSAGES` — check the Content-Type, `express.urlencoded({ extended: true })` or `unflatten(request.form)`; use the separate constructor for placement
- The operator reply did not appear — check that installation is complete, the `event.bind` subscription, HTTPS availability of the handler, and that `data.CONNECTOR` and the external `chat.id` match

After the fix, repeat the corresponding step: registration and subscription — step 1, activation — step 2, sending — step 3. Resending a visitor message creates a new message; add identifier storage for retries and transactions when adapting for production.

## Continue Learning

- [{#T}](../../api-reference/imopenlines/imconnector/imconnector-register.md)
- [{#T}](../../api-reference/imopenlines/imconnector/imconnector-activate.md)
- [{#T}](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md)
- [{#T}](../../api-reference/imopenlines/imconnector/events/on-im-connector-message-add.md)
