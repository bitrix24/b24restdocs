# How to Create an Open Channels Connector for Website Chat

{% note info "" %}

This example works only with application authorization. It will not function with webhooks.

To use the example, configure the CRest class. For detailed information, refer to the article [Loading and Using CRest PHP SDK](../../sdk/crest-php-sdk/index.md).

{% endnote %}

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant uses the official REST documentation.

{% endnote %}

The connector transmits website visitor messages to Bitrix24 Open Channels and operator replies back to the website. A visitor writes in the website chat, and an operator responds from Bitrix24.

{% note warning "" %}

The connector works **only within the context of an application** (OAuth). An incoming webhook will not work: the `imconnector.*` methods require application authorization. The SDKs perform outgoing REST calls; your web server receives incoming events (`ONIMCONNECTORMESSAGEADD`) and connector configuration (placement `SETTING_CONNECTOR`). For SDK initialization using an application token, see the [chatbot example](../chat-bots/index.md#initializing-the-sdk-using-event-data).

{% endnote %}

## Architecture

The integration consists of a server-side component (the application) and a chat widget on the website:

| File                  | Purpose                                                                  | REST Methods                                                                                 |
| --------------------- | ------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------- |
| `function.*`          | Helpers: connector identifier, chat and message storage, line number     | —                                                                                            |
| `install_connector.*` | Installation: connector registration and event subscription              | `imconnector.register`, `event.bind`                                                         |
| `handler.*`           | Connector configuration (placement) and receiving messages from Bitrix24 | `imconnector.activate`, `imconnector.connector.data.set`, `imconnector.send.status.delivery` |
| `ajax.*`              | Data exchange between the widget and Bitrix24                            | `imconnector.send.messages`                                                                  |
| `index.*`             | Website chat widget (frontend)                                           | —                                                                                            |

The connector identifier (`getConnectorID`), chat history storage (`saveMessage`/`getChat`), and line number (`setLine`/`getLine`) represent platform-dependent storage logic; in the examples below, this logic is moved to helpers.

## 1. Installation: Registering a Connector

During application installation, register the connector using the [imconnector.register](../../api-reference/imopenlines/imconnector/imconnector-register.md) method and subscribe to the [OnImConnectorMessageAdd](../../api-reference/imopenlines/imconnector/events/on-im-connector-message-add.md) event using the [event.bind](../../api-reference/events/event-bind.md) method.

In `imconnector.register`, pass: `ID` — the connector identifier, `NAME` — the name, `ICON`/`ICON_DISABLED` — icons (SVG DATA representation), and `PLACEMENT_HANDLER` — the settings handler URL.

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
    // $b24 is built on an application token (see chatbot example)
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
        // event.bind is not part of the typed connector service — calling via the core
        $b24->core->call('event.bind', [
            'event' => 'OnImConnectorMessageAdd',
            'handler' => $handlerUrl,
        ]);
    }
    ```

- Python

    ```python
    # client is built on an application token (see chatbot example)
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

## 2. Handler: Activation and Receiving Messages

Bitrix24 opens the handler in the Open Channel settings (placement `SETTING_CONNECTOR`) and sends the event there `ONIMCONNECTORMESSAGEADD` upon receiving a message from an operator.

**Activating the connector for a line** is done via the [imconnector.activate](../../api-reference/imopenlines/imconnector/imconnector-activate.md) and [imconnector.connector.data.set](../../api-reference/imopenlines/imconnector/imconnector-connector-data-set.md) methods. `LINE` and `ACTIVE_STATUS` are received in `PLACEMENT_OPTIONS`.

{% list tabs %}

- JS

    ```js
    // In the placement handler SETTING_CONNECTOR
    const options = JSON.parse(req.body.PLACEMENT_OPTIONS)
    const line = Number(options.LINE)

    await $b24.actions.v2.call.make({
        method: 'imconnector.activate',
        params: { CONNECTOR: connectorId, LINE: line, ACTIVE: Number(options.ACTIVE_STATUS) },
        requestId: 'connector-activate',
    })

    await $b24.actions.v2.call.make({
        method: 'imconnector.connector.data.set',
        params: { CONNECTOR: connectorId, LINE: line, DATA: { id: `${connectorId}line${line}`, url_im: widgetUri, name: widgetName } },
        requestId: 'connector-data-set',
    })
    ```

- PHP

    ```php
    $options = json_decode($_REQUEST['PLACEMENT_OPTIONS'], true);
    $line = (string)(int)$options['LINE'];

    $b24->getIMOpenLinesScope()->connector()->activate($connectorId, $line, (int)$options['ACTIVE_STATUS']);

    $b24->getIMOpenLinesScope()->connector()->setData($connectorId, $line, [
        'id' => $connectorId . 'line' . $line,
        'url_im' => $widgetUri,
        'name' => $widgetName,
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
        data={"id": f"{connector_id}line{line}", "url_im": widget_uri, "name": widget_name},
    ).response
    ```

{% endlist %}

**Receiving a message from an operator.** Upon the `ONIMCONNECTORMESSAGEADD` event, save the message and confirm delivery using the [imconnector.send.status.delivery](../../api-reference/imopenlines/imconnector/imconnector-send-status-delivery.md) method.

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
                    MESSAGES: [{ im: message.im, message: { id: [messageId] }, chat: { id: message.chat.id } }],
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
                ['im' => $message['im'], 'message' => ['id' => [$messageId]], 'chat' => ['id' => $message['chat']['id']]],
            ]);
        }
    }
    ```

- Python

    ```python
    if request.form.get("event") == "ONIMCONNECTORMESSAGEADD":
        for message in messages:  # data[MESSAGES]  from the event body
            message_id = save_message(message["chat"]["id"],  message)  # local storage
            client.imconnector.send.status.delivery(
                connector=connector_id,
                line=get_line(),
                messages=[{"im": message["im"], "message": {"id": [message_id]}, "chat": {"id": message["chat"]["id"]}}],
            ).response
    ```

{% endlist %}

## 3. AJAX: Sending Visitor Messages to Bitrix24

The website widget sends visitor messages to `ajax.*`, from where they are sent to the Open Channel using the [imconnector.send.messages](../../api-reference/imopenlines/imconnector/imconnector-send-messages.md) method.

The message structure `MESSAGES[]` consists of: `user` (`id`, `name`), `message` (`id`, `date`, `text`), and `chat` (`id`, `url`).

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

## 4. Website Chat Widget

`index.*` serves an HTML page containing the chat: an input field, a message list, and periodic polling of `ajax.*` to load history and operator replies. This is a standard frontend (HTML + JS + fetch to your `ajax.*`), without direct Bitrix24 REST calls — all requests to Bitrix24 are routed through the server-side component.

## 5. Running the Connector

1. Deploy the server files to a public HTTPS URL
2. Create a [Local application](../../settings/app-installation/local-apps/index.md) of the "Server" type with `imopenlines`, `imconnector`, and `im` permissions
3. Open `install_connector.*` to register the connector and subscribe to the event
4. In the **Contact Center**, open the connector `ExampleSiteChat`, select Open Channel and activate — Bitrix24 will call `handler.*` with placement `SETTING_CONNECTOR`
5. Place the widget (`index.*`) on the website and test the message exchange
