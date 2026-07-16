# Example of Creating a Chatbot for Open Channels

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Creating a chatbot for *Open Channels* is similar to [creating a regular chatbot](./index.md), but there are two differences:

1. When registering via [imbot.register](../../api-reference/chat-bots/outdated/bots/imbot-register.md), pass `O` to the `TYPE` parameter. To extend an existing bot, we pass `OPENLINE => Y` — in this case, the bot operates in hybrid mode (Group chat, private chat, and Open Channels).

2. In hybrid mode, check `CHAT_ENTITY_TYPE` in all incoming events ([ONIMBOTMESSAGEADD](../../api-reference/chat-bots/outdated/messages/events/on-imbot-message-add.md) and [ONIMBOTJOINCHAT](../../api-reference/chat-bots/outdated/chats/events/on-imbot-join-chat.md)) — for Open Channels, it equals `LINES`.

A scope of [`imopenlines`](../../api-reference/scopes/permissions.md) is required for tight integration with Open Channels. For SDK initialization using event data, see the [regular chatbot example](./index.md#initializing-the-sdk-using-event-data).

## Registering a Bot for Open Channels

{% list tabs %}

- JS

    ```js
    await $b24.actions.v2.call.make({
        method: 'imbot.register',
        params: {
            CODE: 'OpenLineBot',
            TYPE: 'O',          // bot type for Open Lines
            OPENLINE: 'Y',      // hybrid mode
            EVENT_MESSAGE_ADD: HANDLER_URL,
            EVENT_WELCOME_MESSAGE: HANDLER_URL,
            EVENT_BOT_DELETE: HANDLER_URL,
            PROPERTIES: { NAME: 'Support Line', COLOR: 'GREEN' },
        },
        requestId: 'imbot-register-ol',
    })
    ```

- PHP

    ```php
    // imbot.* is not among the typed PHP services — we call it via the core
    $b24->core->call('imbot.register', [
        'CODE' => 'OpenLineBot',
        'TYPE' => 'O',
        'OPENLINE' => 'Y',
        'EVENT_MESSAGE_ADD' => $handlerUrl,
        'EVENT_WELCOME_MESSAGE' => $handlerUrl,
        'EVENT_BOT_DELETE' => $handlerUrl,
        'PROPERTIES' => ['NAME' => 'Support Line', 'COLOR' => 'GREEN'],
    ]);
    ```

- Python

    ```python
    client.imbot.register(
        code="OpenLineBot",
        properties={"NAME": "Support Line", "COLOR": "GREEN"},
        event_message_add=HANDLER_URL,
        event_welcome_message=HANDLER_URL,
        event_bot_delete=HANDLER_URL,
        type="O",
        openline=True,
    ).response
    ```

{% endlist %}

## Checking the Chat Type in a Handler

In hybrid mode, process messages from Open Channels separately using the `CHAT_ENTITY_TYPE` field.

{% list tabs %}

- JS

    ```js
    if (data.PARAMS.CHAT_ENTITY_TYPE === 'LINES') {
        // message from an Open Line — first-line support logic
    }
    ```

- PHP

    ```php
    if (($data['PARAMS']['CHAT_ENTITY_TYPE'] ?? '') === 'LINES') {
        // message from an Open Line
    }
    ```

- Python

    ```python
    if data.get("data[PARAMS][CHAT_ENTITY_TYPE]") == "LINES":
        # message from an Open Line
        ...
    ```

{% endlist %}

## Session Management

With the `imopenlines` permission, the following commands for conversation management are available:

- [imopenlines.bot.session.operator](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-operator.md) — transfer to an available operator
- [imopenlines.bot.session.transfer](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-transfer.md) — transfer to a specific operator
- [imopenlines.bot.session.finish](../../api-reference/imopenlines/openlines/chat-bots/imopenlines-bot-session-finish.md) — finish the session

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
        params: { CHAT_ID: chatId, USER_ID: operatorId },
        requestId: 'session-transfer',
    })

    // End session
    await $b24.actions.v2.call.make({
        method: 'imopenlines.bot.session.finish',
        params: { CHAT_ID: chatId },
        requestId: 'session-finish',
    })
    ```

- PHP

    ```php
    $b24->core->call('imopenlines.bot.session.operator', ['CHAT_ID' => $chatId]);
    $b24->core->call('imopenlines.bot.session.transfer', ['CHAT_ID' => $chatId, 'USER_ID' => $operatorId]);
    $b24->core->call('imopenlines.bot.session.finish', ['CHAT_ID' => $chatId]);
    ```

- Python

    ```python
    client.imopenlines.bot.session.operator(chat_id=chat_id).response
    client.imopenlines.bot.session.transfer(chat_id=chat_id, user_id=operator_id).response
    client.imopenlines.bot.session.finish(chat_id=chat_id).response
    ```

{% endlist %}

## Ready-to-Use Example: ITR Bot

Use "ITR Bot" as an example of an Open Channels bot with a multi-level menu: it can be [downloaded from GitHub](https://github.com/bitrix24com/bots) (file `itr.php`) or found in the boxed version in the folder `\Bitrix\ImBot\Bot\OpenlinesMenuExample`.

The bot acts as the first line of Helpdesk: messages are first received by the bot, and then passed to operators in an enqueued state. A customer can switch to an operator at any time by sending `0`.
