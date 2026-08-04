# How to Create an Open Channels Connector for Website Chat

> Scope: [`imopenlines`, `imconnector`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: any application user

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant uses the official REST documentation.

{% endnote %}

The connector transmits website visitor messages to Bitrix24 Open Channels and operator replies back to the website. A visitor writes in the website chat, and an operator responds from Bitrix24.

{% note warning "" %}

The connector works **only within the context of an application** (OAuth). An incoming webhook will not work: the `imconnector.*` methods require application authorization.

SDKs perform outgoing method calls. Your web server receives incoming events (`ONIMCONNECTORMESSAGEADD`) and connector settings (placement `SETTING_CONNECTOR`).

{% endnote %}

## Preparation

For the scenario, you need:

- a local application of the `Server` type with the `imopenlines`, `imconnector`, and `im` permissions
- a public HTTPS URL for the application server side
- URL of the installation handler `install_connector.*`
- URL of the event and settings handler `handler.*`
- HTML page of the chat widget `index.*`

In the examples, replace:

- `example_site_chat` with your connector code
- `https://your-domain.example/handler` with the event and settings handler URL
- `widgetUri` with the chat widget URL
- `widgetName` with the channel name

Before the first method call, initialize the SDK with the application OAuth token. In the examples, `$b24` and `client` are already initialized clients that perform calls on behalf of the application.

## Initialize the SDK in an Application Context

Bitrix24 passes authorization in requests to application handlers. Use the `auth` object to create an SDK client before calling `imconnector.*` and `event.bind`.

{% list tabs %}

- JS

    ```js
    // npm install express @bitrix24/b24jssdk
    import { B24OAuth } from '@bitrix24/b24jssdk'

    const APP = { clientId: 'local.xxxxxxxx.xxxxxxxx', clientSecret: 'yyyyyyyy' }

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

    const $b24 = makeClient(req.body.auth)
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
    use Symfony\Component\HttpFoundation\Request;

    $request = Request::createFromGlobals();
    $appProfile = ApplicationProfile::initFromArray([
        'BITRIX24_PHP_SDK_APPLICATION_CLIENT_ID' => 'local.xxxxxxxx.xxxxxxxx',
        'BITRIX24_PHP_SDK_APPLICATION_CLIENT_SECRET' => 'yyyyyyyy',
        'BITRIX24_PHP_SDK_APPLICATION_SCOPE' => 'imopenlines,imconnector,im',
    ]);

    $authToken = AuthToken::initFromEventRequest($request);
    $domain = (string)$request->request->all('auth')['domain'];

    $log = new Logger('openlines');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $b24 = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->init($appProfile, $authToken, $domain, DefaultOAuthServerUrl::default());
    ```

- Python

    ```python
    # pip install b24pysdk flask
    from flask import request
    from b24pysdk import BitrixApp, BitrixToken, Client

    APP = BitrixApp(client_id="local.xxxxxxxx.xxxxxxxx", client_secret="yyyyyyyy")

    def make_client(auth: dict) -> tuple[Client, BitrixToken]:
        token = BitrixToken(
            domain=auth["domain"],
            auth_token=auth["access_token"],
            refresh_token=auth.get("refresh_token", ""),
            bitrix_app=APP,
        )
        return Client(token), token

    auth = request.json["auth"]  # auth dictionary from the handler request body
    client, token = make_client(auth)
    ```

{% endlist %}

## Architecture

The integration consists of a server-side component (the application) and a chat widget on the website:

| File | Purpose | Methods |
|---|---|---|
| `function.*` | Helpers: connector identifier, chat and message storage, line number | - |
| `install_connector.*` | Installation: connector registration and event subscription | `imconnector.register`, `event.bind` |
| `handler.*` | Connector settings (placement) and receiving messages from Bitrix24 | `imconnector.activate`, `imconnector.connector.data.set`, `imconnector.send.status.delivery` |
| `ajax.*` | Data exchange between the widget and Bitrix24 | `imconnector.send.messages` |
| `index.*` | Website chat widget (frontend) | - |

The connector identifier (`getConnectorID`), chat history storage (`saveMessage`/`getChat`), and line number (`setLine`/`getLine`) are platform-dependent storage logic; in the examples below, this logic is moved to helpers.

Minimal helper logic:

| Helper | What it does | What data it stores |
|---|---|---|
| `getConnectorID` | Returns a permanent connector code | `example_site_chat` or another code from the `ID` parameter of `imconnector.register` |
| `setLine` / `getLine` | Saves and returns the Open Channel ID | `LINE` from `PLACEMENT_OPTIONS` |
| `saveMessage` | Saves the operator message in the external chat history | `message.id`, `chat.id`, `im.chat_id`, `im.message_id` |
| `getChat` | Returns message history for the widget | external chat ID and message list |

The scenario consists of five steps.

1. Register the connector using [imconnector.register](../../api-reference/imopenlines/imconnector/imconnector-register.md)
2. Subscribe the application to the [OnImConnectorMessageAdd](../../api-reference/imopenlines/imconnector/events/on-im-connector-message-add.md) event using [event.bind](../../api-reference/events/event-bind.md)
3. Activate the connector for an Open Channel using [imconnector.activate](../../api-reference/imopenlines/imconnector/imconnector-activate.md) and [imconnector.connector.data.set](../../api-reference/imopenlines/imconnector/imconnector-connector-data-set.md)
4. Send visitor messages using [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md)
5. Receive operator replies in the event handler and confirm delivery using [imconnector.send.status.delivery](../../api-reference/imopenlines/imconnector/imconnector-send-status-delivery.md)

## 1. Installation: Register a Connector

During application installation, register the connector using the [imconnector.register](../../api-reference/imopenlines/imconnector/imconnector-register.md) method and subscribe to the [OnImConnectorMessageAdd](../../api-reference/imopenlines/imconnector/events/on-im-connector-message-add.md) event using the [event.bind](../../api-reference/events/event-bind.md) method.

In `imconnector.register`, pass: `ID` - the connector identifier, `NAME` - the name, `ICON`/`ICON_DISABLED` - icons (SVG DATA representation), and `PLACEMENT_HANDLER` - the settings handler URL.

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    const connectorId = 'example_site_chat'
    const handlerUrl = 'https://your-domain.example/handler'
    const icon = {
        DATA_IMAGE: 'data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20viewBox%3D%220%200%2070%2071%22%3E%3C/svg%3E',
        COLOR: '#a6ffa3', SIZE: '100%', POSITION: 'center',
    }

    const reg = await $b24.actions.v2.call.make({
        method: 'imconnector.register',
        params: { ID: connectorId, NAME: 'ExampleSiteChat', ICON: icon, ICON_DISABLED: { ...icon, COLOR: '#ffb3a3' }, PLACEMENT_HANDLER: handlerUrl },
        requestId: 'connector-register',
    })

    if (reg.getData().result) {
        await $b24.actions.v2.call.make({
            method: 'event.bind',
            params: { event: 'OnImConnectorMessageAdd', handler: handlerUrl },
            requestId: 'event-bind',
        })
    }
    ```

- PHP

    ```php
    <?php
    // $b24 is built on an application token
    $connectorId = 'example_site_chat';
    $handlerUrl = 'https://your-domain.example/handler';
    $icon = [
        'DATA_IMAGE' => 'data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20viewBox%3D%220%200%2070%2071%22%3E%3C/svg%3E',
        'COLOR' => '#a6ffa3', 'SIZE' => '100%', 'POSITION' => 'center',
    ];

    $reg = $b24->getIMOpenLinesScope()->connector()->register([
        'ID' => $connectorId,
        'NAME' => 'ExampleSiteChat',
        'ICON' => $icon,
        'ICON_DISABLED' => array_merge($icon, ['COLOR' => '#ffb3a3']),
        'PLACEMENT_HANDLER' => $handlerUrl,
    ]);

    if ($reg->isSuccess()) {
        // event.bind is not part of the typed connector service, so call it through the core
        $b24->core->call('event.bind', [
            'event' => 'OnImConnectorMessageAdd',
            'handler' => $handlerUrl,
        ]);
    }
    ```

- Python

    ```python
    # client is built on an application token
    connector_id = "example_site_chat"
    handler_url = "https://your-domain.example/handler"
    icon = {
        "DATA_IMAGE": "data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20viewBox%3D%220%200%2070%2071%22%3E%3C/svg%3E",
        "COLOR": "#a6ffa3", "SIZE": "100%", "POSITION": "center",
    }

    reg = client.imconnector.register(
        bitrix_id=connector_id,
        name="ExampleSiteChat",
        icon=icon,
        placement_handler=handler_url,
        icon_disabled={**icon, "COLOR": "#ffb3a3"},
    ).response

    if reg.result:
        client.event.bind(event="OnImConnectorMessageAdd", handler=handler_url).response
    ```

{% endlist %}

After successful connector registration and event subscription, the methods return `true`.

```json
{
    "result": true
}
```

## 2. Handler: Activation and Receiving Messages

Bitrix24 opens the handler in the Open Channel settings (placement `SETTING_CONNECTOR`) and sends the `ONIMCONNECTORMESSAGEADD` event there when an operator sends a message.

**Activating the connector for a line** is done through [imconnector.activate](../../api-reference/imopenlines/imconnector/imconnector-activate.md) and [imconnector.connector.data.set](../../api-reference/imopenlines/imconnector/imconnector-connector-data-set.md). `LINE` and `ACTIVE_STATUS` come in `PLACEMENT_OPTIONS`.

In the `DATA` parameter of `imconnector.connector.data.set`, pass the external channel settings:

- `ID` - chat or channel ID in the external system
- `URL_IM` - chat link for the operator interface
- `NAME` - channel name

{% list tabs %}

- JS

    ```js
    // In the SETTING_CONNECTOR placement handler
    const options = JSON.parse(req.body.PLACEMENT_OPTIONS)
    const line = Number(options.LINE)

    await $b24.actions.v2.call.make({
        method: 'imconnector.activate',
        params: { CONNECTOR: connectorId, LINE: line, ACTIVE: Number(options.ACTIVE_STATUS) },
        requestId: 'connector-activate',
    })

    await $b24.actions.v2.call.make({
        method: 'imconnector.connector.data.set',
        params: { CONNECTOR: connectorId, LINE: line, DATA: { ID: `${connectorId}_line_${line}`, URL_IM: widgetUri, NAME: widgetName } },
        requestId: 'connector-data-set',
    })
    ```

- PHP

    ```php
    $options = json_decode($_REQUEST['PLACEMENT_OPTIONS'], true);
    $line = (string)(int)$options['LINE'];

    $b24->getIMOpenLinesScope()->connector()->activate($connectorId, $line, (int)$options['ACTIVE_STATUS']);

    $b24->getIMOpenLinesScope()->connector()->setData($connectorId, $line, [
        'ID' => $connectorId . '_line_' . $line,
        'URL_IM' => $widgetUri,
        'NAME' => $widgetName,
    ]);
    ```

- Python

    ```python
    import json
    options = json.loads(request.form["PLACEMENT_OPTIONS"])
    line = int(options["LINE"])

    client.imconnector.activate(connector=connector_id, line=line, active=int(options["ACTIVE_STATUS"])).response

    client.imconnector.connector.data.set(
        connector=connector_id,
        line=line,
        data={"ID": f"{connector_id}_line_{line}", "URL_IM": widget_uri, "NAME": widget_name},
    ).response
    ```

{% endlist %}

After activating the connector and saving channel settings, the methods return `true`.

```json
{
    "result": true
}
```

**Receiving a message from an operator.** On the `ONIMCONNECTORMESSAGEADD` event, save the message and confirm delivery using [imconnector.send.status.delivery](../../api-reference/imopenlines/imconnector/imconnector-send-status-delivery.md).

To confirm delivery, use the `im` object from the event. It contains the internal `chat_id` and `message_id` identifiers. Pass the external `message.id` returned by `saveMessage` as a separate array in `message.id`.

{% list tabs %}

- JS

    ```js
    if (req.body.event === 'ONIMCONNECTORMESSAGEADD' && req.body.data.CONNECTOR === connectorId) {
        for (const message of req.body.data.MESSAGES) {
            const messageId = saveMessage(message.chat.id, message) // local storage
            await $b24.actions.v2.call.make({
                method: 'imconnector.send.status.delivery',
                params: {
                    CONNECTOR: connectorId,
                    LINE: getLine(),
                    MESSAGES: [{
                        im: { chat_id: message.im.chat_id, message_id: message.im.message_id },
                        message: { id: [messageId], date: Math.floor(Date.now() / 1000) },
                        chat: { id: message.chat.id },
                    }],
                },
                requestId: 'status-delivery',
            })
        }
    }
    ```

- PHP

    ```php
    if (($_REQUEST['event'] ?? '') === 'ONIMCONNECTORMESSAGEADD'
        && ($_REQUEST['data']['CONNECTOR'] ?? '') === $connectorId) {
        foreach ($_REQUEST['data']['MESSAGES'] as $message) {
            $messageId = saveMessage($message['chat']['id'], $message); // local storage
            $b24->getIMOpenLinesScope()->connector()->sendStatusDelivery($connectorId, getLine(), [
                [
                    'im' => [
                        'chat_id' => $message['im']['chat_id'],
                        'message_id' => $message['im']['message_id'],
                    ],
                    'message' => ['id' => [$messageId], 'date' => time()],
                    'chat' => ['id' => $message['chat']['id']],
                ],
            ]);
        }
    }
    ```

- Python

    ```python
    import time

    if request.form.get("event") == "ONIMCONNECTORMESSAGEADD":
        for message in messages:  # data[MESSAGES] from the event body
            message_id = save_message(message["chat"]["id"], message)  # local storage
            client.imconnector.send.status.delivery(
                connector=connector_id,
                line=get_line(),
                messages=[{
                    "im": {"chat_id": message["im"]["chat_id"], "message_id": message["im"]["message_id"]},
                    "message": {"id": [message_id], "date": int(time.time())},
                    "chat": {"id": message["chat"]["id"]},
                }],
            ).response
    ```

{% endlist %}

A successful delivery confirmation returns `SUCCESS: true`.

```json
{
    "result": {
        "SUCCESS": true,
        "DATA": []
    }
}
```

## 3. AJAX: Sending Visitor Messages to Bitrix24

The website widget sends visitor messages to `ajax.*`. The server side sends them to the Open Channel using [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md).

The `MESSAGES[]` message structure consists of `user` (`id`, `name`), `message` (`id`, `date`, `text`), and `chat` (`id`, `url`).

{% list tabs %}

- JS

    ```js
    const arMessage = {
        user: { id: chatId, name: visitorName },
        message: { id: messageId, date: Math.floor(Date.now() / 1000), text: visitorText },
        chat: { id: chatId, url: pageUrl },
    }

    await $b24.actions.v2.call.make({
        method: 'imconnector.send.messages',
        params: { CONNECTOR: connectorId, LINE: lineId, MESSAGES: [arMessage] },
        requestId: 'send-messages',
    })
    ```

- PHP

    ```php
    $arMessage = [
        'user' => ['id' => $chatID, 'name' => htmlspecialchars($_POST['name'])],
        'message' => ['id' => $messageId, 'date' => time(), 'text' => htmlspecialchars($_POST['message'])],
        'chat' => ['id' => $chatID, 'url' => htmlspecialchars($_SERVER['HTTP_REFERER'])],
    ];

    $b24->getIMOpenLinesScope()->connector()->sendMessages($connectorId, $lineId, [$arMessage]);
    ```

- Python

    ```python
    import time
    ar_message = {
        "user": {"id": chat_id, "name": visitor_name},
        "message": {"id": message_id, "date": int(time.time()), "text": visitor_text},
        "chat": {"id": chat_id, "url": page_url},
    }

    client.imconnector.send.messages(connector=connector_id, line=line_id, messages=[ar_message]).response
    ```

{% endlist %}

In the response, save `session.CHAT_ID` and `session.ID`. They confirm that the message reached the Open Channel.

```json
{
    "result": {
        "SUCCESS": true,
        "DATA": {
            "RESULT": [
                {
                    "SUCCESS": true,
                    "session": {
                        "ID": "323",
                        "CHAT_ID": "1767"
                    }
                }
            ]
        }
    }
}
```

## 4. Website Chat Widget

`index.*` returns an HTML page with the chat: an input field, a message list, and periodic polling of `ajax.*` to load history and operator replies. This is a regular frontend (HTML + JS + fetch to your `ajax.*`), without direct Bitrix24 method calls. All Bitrix24 requests go through the server side.

## 5. Running the Connector

1. Deploy the server files to a public HTTPS URL
2. Create a [Local application](../../settings/app-installation/local-apps/index.md) of the `Server` type with the `imopenlines`, `imconnector`, and `im` permissions
3. Open `install_connector.*` to register the connector and subscribe to the event
4. In the **Contact Center**, open the `ExampleSiteChat` connector, select an Open Channel, and activate it. Bitrix24 will call `handler.*` with placement `SETTING_CONNECTOR`
5. Place the widget (`index.*`) on the website and test message exchange

## Check the Result

Send a message from the widget on the website. A conversation should open in Bitrix24 in the selected Open Channel. The operator's reply should appear in the website chat history.

Check that the server side stores:

- `CONNECTOR` - connector code
- `LINE` - Open Channel ID
- `session.CHAT_ID` from the `imconnector.send.messages` response
- `im.chat_id` and `im.message_id` from the `ONIMCONNECTORMESSAGEADD` event

## Errors and Diagnostics

If a method returns an error, check the request data.

- `WRONG_AUTH_TYPE` - the method was called outside an OAuth application context
- `ERROR_ARGUMENT` - a required parameter `CONNECTOR`, `LINE`, `MESSAGES`, `DATA`, or `ACTIVE` was not passed
- `ERROR_EVENT_NOT_FOUND` - an invalid event code was passed to `event.bind`
- `NOT_ACTIVE_LINE` - the line is inactive or does not exist

If the `ONIMCONNECTORMESSAGEADD` event does not arrive, check that application installation is complete, the `handler.*` endpoint is available over HTTPS, and the event is registered through `event.bind`.

After fixing the error, repeat the scenario from the step where execution stopped:

- connector registration or event subscription error: repeat step 1
- activation or channel settings saving error: repeat step 2
- visitor message sending error: repeat step 3
- operator reply delivery to the external chat error: process the `ONIMCONNECTORMESSAGEADD` event again

## Important Notes

- `imconnector.*` methods work only within an OAuth application context
- The event handler and placement handler URL must be available from the internet over HTTPS
- Events will start arriving only after application installation is complete
- Running `install_connector.*` again with the same `ID` updates the existing connector

## Continue Learning

- [{#T}](../../api-reference/imopenlines/imconnector/imconnector-register.md)
- [{#T}](../../api-reference/imopenlines/imconnector/imconnector-activate.md)
- [{#T}](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md)
- [{#T}](../../api-reference/imopenlines/imconnector/events/on-im-connector-message-add.md)
