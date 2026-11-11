# Event Formats for imbot.v2

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who receives events: the application that registered the bot, or a webhook with the same `botToken`

Where to find the event type and its data depends on the delivery mode:

- `fetch` — the [imbot.v2.Event.get](./event-get.md) response: the type is in the `result.events[].type` field, the data is in `result.events[].data`
- `webhook` — a request to the bot URL: the type is in the `event` field, the data is in `data`

How to choose a mode is described in the [Event Delivery Modes](./index.md#event-modes) overview.

The examples are shortened and show the data in `fetch` mode, while the `ONIMBOTV2DELETE` example shows it in `webhook` mode. The `bot` and `user` objects are shown most fully in the [ONIMBOTV2MESSAGEADD](#onimbotv2messageadd) example. All fields of `message`, `chat`, and `user` are described in the [{#T}](../../entities.md) article.

## Automatic Subscription Management

Bitrix24 manages the bot's subscription to `ONIMBOTV2*` events itself:

- creates it on [imbot.v2.Bot.register](../bots/bot-register.md) with `fields.eventMode: "webhook"` and the required `fields.webhookUrl`
- adds a subscription for the new address on [imbot.v2.Bot.update](../bots/bot-update.md) if a new `fields.webhookUrl` is passed
- removes it on [imbot.v2.Bot.unregister](../bots/bot-unregister.md) or when switching to `fetch` mode, if the application has no other bots left: subscriptions are shared by all bots of the application

An event of any bot is sent to the `webhookUrl` of all bots of the application. Therefore, after the address is changed or the bot is removed, the previous address keeps receiving events as long as the application has other bots.

If you switch a bot from `fetch` mode back to `webhook`, pass `fields.webhookUrl` together with `fields.eventMode`. Without an address, the subscription is not created, and the bot stops receiving events if the application has no other bots with a subscription.

Do not call [event.bind](../../../../events/event-bind.md) and [event.unbind](../../../../events/event-unbind.md) manually: otherwise, the subscriptions will diverge from those managed by Bitrix24.

## Which Events to Handle First

The minimum set of events for a working bot:

- [ONIMBOTV2MESSAGEADD](#onimbotv2messageadd) — incoming messages from users
- [ONIMBOTV2COMMANDADD](#onimbotv2commandadd) — calls to slash commands
- [ONIMBOTV2JOINCHAT](#onimbotv2joinchat) — the bot being added to a chat (usually sends a greeting)
- [ONIMBOTV2DELETE](#onimbotv2delete) — the bot being deleted (cleaning up resources), only in `webhook` mode

Additionally, based on the scenario:

- [ONIMBOTV2MESSAGEUPDATE](#onimbotv2messageupdate) and [ONIMBOTV2MESSAGEDELETE](#onimbotv2messagedelete) — if you support editing/deleting
- [ONIMBOTV2REACTIONCHANGE](#onimbotv2reactionchange) — if you track reactions
- [ONIMBOTV2CONTEXTGET](#onimbotv2contextget) — if you use the incoming context of the dialogue

## Bot Object Format {#bot-format}

The content of the `bot` field depends on the event delivery mode.

- **`fetch`** (`imbot.v2.Event.get`) — full bot object, as in the response from [imbot.v2.Bot.get](../bots/bot-get.md)
- **`webhook`** — the `{id, code}` object, plus `auth` if the bot was registered by an application. `auth` contains OAuth tokens that the handler uses to call methods in response to the event, for example to send a message

Each webhook call contains data for **one** bot. If the application has registered multiple bots and the event concerns several of them, the webhook is called separately for each.

Example of the `bot` object in webhook mode. The values come as strings, as described in the "Data Types in Webhook Mode" section; the other data fields are omitted in the example:

```json
{
    "bot": {
        "id": "456",
        "code": "support_bot",
        "auth": {
            "access_token": "0b02a0690000071b0008440001b0007a16b39202c2490f015",
            "expires": "1772093963",
            "expires_in": "3600",
            "scope": "imbot",
            "domain": "some-domain.bitrix24.com",
            "server_endpoint": "https://oauth.bitrix.info/rest/",
            "status": "F",
            "client_endpoint": "https://some-domain.bitrix24.com/rest/",
            "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
            "user_id": "456",
            "application_token": "831c76b092f9f135d9b6b36c3a720757"
        }
    }
}
```

### Webhook Notification Structure {#webhook-payload}

Bitrix24 sends a POST request with the following fields to the bot's `webhookUrl`:

#|
|| **Field** | **Type** | **Description** ||
|| **event** | `string` | Event type, for example `ONIMBOTV2MESSAGEADD` ||
|| **event_handler_id** | `string` | ID of the event handler in Bitrix24 ||
|| **data** | `object` | Event data. The contents depend on the event and are described below ||
|| **ts** | `string` | Time the event was sent, Unix timestamp ||
|| **auth** | `object` | Bitrix24 data — `domain`, `client_endpoint`, `server_endpoint`, `member_id` — and `application_token` for verifying the request. For details, see [below](#auth) ||
|#

The request may contain **two different `auth` objects**:

- **top-level `auth`** — Bitrix24 data and a token for verifying the request, always present
- **`data.bot.auth`** — the bot's OAuth tokens for calling methods in response to the event. Present only if the bot was registered by an application

The example shows only the `bot` object from `data`:

```json
{
    "event": "ONIMBOTV2MESSAGEADD",
    "event_handler_id": "1335",
    "data": {
        "bot": {
            "id": "456",
            "code": "support_bot",
            "auth": {"access_token": "...", "refresh_token": "...", "application_token": "..."}
        }
    },
    "ts": "1772093963",
    "auth": {
        "domain": "your-account.bitrix24.com",
        "application_token": "..."
    }
}
```

{% note warning "" %}

To verify that the request was sent by Bitrix24, check the `application_token` from the **top level** — `auth.application_token`, not `data.bot.auth.application_token`.

{% endnote %}

The request arrives as `application/x-www-form-urlencoded`, not JSON: the example above shows only the data structure. The keys are written in PHP format, as encoded by `http_build_query`: `data[bot][id]=...`, `auth[application_token]=...`.

### Chat Object in Events {#chat-in-events}

The `chat` object in events does not contain the fields `role` and `muteList` — these fields depend on the specific user and cannot be the same for all event recipients.

### Auth Parameter {#auth}

In bot events, the top-level `auth` contains only `domain`, `client_endpoint`, `server_endpoint`, `member_id`, and `application_token`. Access tokens come in `data.bot.auth`. The table below is the general list of `auth` keys for all Bitrix24 events.

{% include notitle [Table with keys in the auth array](../../../../../_includes/auth-params-in-events.md) %}

{% note info "" %}

Whether the `bot.auth` field is present depends on the delivery mode and on who registered the bot:

- `webhook` — the field is present if the bot was registered by an application. A bot registered via an inbound webhook does not have this field: it calls methods with the same webhook
- `fetch` — the field is absent: the application or webhook calls `imbot.v2.Event.get` and other methods with its own authorization, so separate tokens are not needed

{% endnote %}

## Data Types in Webhook Mode

In `webhook` mode, **all scalar values come as strings**: numbers, `true`, and `false`. Fields with a `null` value and empty arrays are not included in the request.

#|
|| **Type** | **Value in `fetch`** | **Value in `webhook`** ||
|| `integer` | `789` | `"789"` ||
|| `boolean` | `true` / `false` | `"1"` / `"0"` ||
|| ```string|false``` | `false` | `"0"` ||
|| `null` | `null` | The field is absent from the request ||
|| Empty array | `[]` | The field is absent from the request ||
|#

When processing events in `webhook` mode, cast types explicitly: `(int)$data['message']['id']`, `$data['user']['active'] !== '0'`. Treat a missing field as `null` or an empty array, for example `$data['message']['params'] ?? []`. In `fetch` mode, types correspond to the documentation.

---

## ONIMBOTV2MESSAGEADD {#onimbotv2messageadd}

A user wrote to the bot: in a one-on-one chat, with a bot mention, or as a reply to its message. Bots of type `personal` and `supervisor` receive all user messages in chats they belong to, except messages from other bots.

#|
|| **Field** | **Type** | **Description** ||
|| **bot** | `object` | [Bot object](#bot-format) ||
|| **message** | [`Message`](../../entities.md#message) | The sent message ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat where the message was sent ||
|| **user** | [`User`](../../entities.md#user) | The author of the message ||
|| **language** | `string` | Bitrix24 language (e.g., `en`, `de`) ||
|| **additionalMessages** | `array` | Array of [`Message`](../../entities.md#message) objects: the original message if `message` is a reply to it. If it is not a reply, an empty array is returned, and in `webhook` mode the field is absent ||
|#

### Example Data

```json
{
    "bot": {
        "id": 456,
        "code": "support_bot",
        "type": "bot",
        "isHidden": false,
        "isSupportOpenline": false,
        "isReactionsEnabled": true,
        "backgroundId": null,
        "language": "en",
        "moduleId": "rest",
        "eventMode": "fetch",
        "countMessage": 150,
        "countCommand": 3,
        "countChat": 12,
        "countUser": 45
    },
    "message": {
        "id": 789,
        "chatId": 5,
        "authorId": 1,
        "date": "2025-01-15T10:30:00+02:00",
        "text": "[USER=456]Support Bot[/USER] Hello!",
        "isSystem": false,
        "uuid": null,
        "forward": null,
        "params": [],
        "viewedByOthers": false
    },
    "chat": {
        "id": 5,
        "dialogId": "chat5",
        "type": "chat",
        "name": "Support Chat",
        "entityType": "",
        "owner": 1,
        "avatar": "",
        "color": "#ab7761"
    },
    "user": {
        "id": 1,
        "active": true,
        "name": "John Smith",
        "firstName": "John",
        "lastName": "Smith",
        "workPosition": "Developer",
        "color": "#df532d",
        "avatar": "",
        "gender": "M",
        "birthday": "",
        "extranet": false,
        "bot": false,
        "connector": false,
        "externalAuthId": "default",
        "status": "online",
        "idle": false,
        "lastActivityDate": false,
        "absent": false,
        "departments": [1],
        "phones": false,
        "type": "user"
    },
    "language": "en",
    "additionalMessages": []
}
```

---

## ONIMBOTV2MESSAGEUPDATE {#onimbotv2messageupdate}

A message has been edited. Bitrix24 selects the recipients by the same rules as for [ONIMBOTV2MESSAGEADD](#onimbotv2messageadd).

#|
|| **Field** | **Type** | **Description** ||
|| **bot** | `object` | [Bot object](#bot-format) ||
|| **message** | [`Message`](../../entities.md#message) | The updated message ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat where the message was edited ||
|| **user** | [`User`](../../entities.md#user) | The author of the message ||
|| **language** | `string` | Bitrix24 language ||
|| **additionalMessages** | `array` | Array of [`Message`](../../entities.md#message) objects: the original message if `message` is a reply to it. If it is not a reply, an empty array is returned, and in `webhook` mode the field is absent ||
|#

The data format is identical to [ONIMBOTV2MESSAGEADD](#onimbotv2messageadd). The `message` field contains the updated text.

---

## ONIMBOTV2MESSAGEDELETE {#onimbotv2messagedelete}

A message has been deleted. Bitrix24 selects the recipients by the same rules as for [ONIMBOTV2MESSAGEADD](#onimbotv2messageadd).

#|
|| **Field** | **Type** | **Description** ||
|| **bot** | `object` | [Bot object](#bot-format) ||
|| **messageId** | `integer` | ID of the deleted message ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat where the message was deleted ||
|| **user** | [`User`](../../entities.md#user) | The author of the deleted message ||
|| **language** | `string` | Bitrix24 language ||
|#

### Example Data

```json
{
    "bot": {
        "id": 456,
        "code": "support_bot",
        "type": "bot",
        "eventMode": "fetch"
    },
    "messageId": 789,
    "chat": {
        "id": 112,
        "dialogId": "129",
        "type": "private"
    },
    "user": {
        "id": 129,
        "name": "Anna Schmidt"
    },
    "language": "en"
}
```

---

## ONIMBOTV2JOINCHAT {#onimbotv2joinchat}

The bot has been added to a chat or received an invitation.

#|
|| **Field** | **Type** | **Description** ||
|| **bot** | `object` | [Bot object](#bot-format) ||
|| **dialogId** | `string` | ID of the dialog (e.g., `chat5`) ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat to which the bot was added ||
|| **user** | [`User`](../../entities.md#user) | The user who added the bot ||
|| **language** | `string` | Bitrix24 language ||
|#

### Example Data

```json
{
    "bot": {
        "id": 456,
        "code": "support_bot",
        "type": "bot",
        "eventMode": "fetch"
    },
    "dialogId": "chat5",
    "chat": {
        "id": 5,
        "dialogId": "chat5",
        "type": "chat",
        "name": "Project Chat"
    },
    "user": {
        "id": 1,
        "name": "John Smith",
        "bot": false,
        "type": "user"
    },
    "language": "en"
}
```

---

## ONIMBOTV2DELETE {#onimbotv2delete}

The bot has been deleted from the system. In `webhook` mode, this is the last event the bot receives. In `fetch` mode, the event is not sent: Bitrix24 deletes the queue together with the bot. After the bot is deleted, [imbot.v2.Event.get](./event-get.md) returns the `BOT_NOT_FOUND` error.

#|
|| **Field** | **Type** | **Description** ||
|| **bot** | `object` | [Bot object](#bot-format) ||
|#

### Example Data

```json
{
    "bot": {
        "id": "456",
        "code": "support_bot",
        "auth": {
            "access_token": "...",
            "refresh_token": "...",
            "application_token": "..."
        }
    }
}
```

---

## ONIMBOTV2CONTEXTGET {#onimbotv2contextget}

The user has opened a dialog with the bot, and the bot has received arbitrary data — the context. The context is set when the dialog is opened, for example in a link with the `BOT_CONTEXT` parameter. For details, see the article [{#T}](../bot-context.md).

#|
|| **Field** | **Type** | **Description** ||
|| **bot** | `object` | [Bot object](#bot-format) ||
|| **dialogId** | `string` | ID of the dialog from which the context was received. In a one-on-one chat with the bot — the numeric ID of the user, in a group chat — for example, `chat5` ||
|| **context** | `object` | Arbitrary data passed when opening the dialog ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat ||
|| **user** | [`User`](../../entities.md#user) | The user who opened the dialog ||
|| **language** | `string` | Bitrix24 language ||
|#

### Example Data

```json
{
    "bot": {
        "id": 456,
        "code": "support_bot",
        "type": "bot",
        "eventMode": "fetch"
    },
    "dialogId": "1",
    "context": {
        "entityId": 164,
        "entityType": "task",
        "source": "link"
    },
    "chat": {
        "id": 5,
        "dialogId": "1",
        "type": "private",
        "name": ""
    },
    "user": {
        "id": 1,
        "name": "John Smith",
        "bot": false,
        "type": "user"
    },
    "language": "en"
}
```

---

## ONIMBOTV2COMMANDADD {#onimbotv2commandadd}

The user has invoked a slash command for the bot.

#|
|| **Field** | **Type** | **Description** ||
|| **bot** | `object` | [Bot object](#bot-format) ||
|| **command** | `object` | Command data. Field descriptions — [below](#command-object) ||
|| **message** | [`Message`](../../entities.md#message) | The message with the command ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat from which the command was invoked ||
|| **user** | [`User`](../../entities.md#user) | The user who invoked the command ||
|| **language** | `string` | Bitrix24 language ||
|| **additionalMessages** | `array` | Array of [`Message`](../../entities.md#message) objects: the original message if `message` is a reply to it. If it is not a reply, an empty array is returned, and in `webhook` mode the field is absent ||
|#

### Command Object {#command-object}

#|
|| **Field** | **Type** | **Description** ||
|| **id** | `integer` | ID of the registered command ||
|| **command** | `string` | The invoked command (e.g., `/help`) ||
|| **params** | `string` | Command parameters — text following the command ||
|| **context** | `string` | How the command was called:

- `textarea` — entered manually
- `keyboard` — a bot keyboard button under a message was pressed
- `menu` — selected from the context menu ||
|#

{% note info "" %}

If a single message contains multiple slash commands, the bot receives a separate event for each.

{% endnote %}

### Example Data

```json
{
    "bot": {
        "id": 456,
        "code": "support_bot",
        "type": "bot",
        "eventMode": "fetch"
    },
    "command": {
        "id": 78,
        "command": "/help",
        "params": "topic",
        "context": "textarea"
    },
    "message": {
        "id": 790,
        "chatId": 5,
        "authorId": 1,
        "date": "2025-01-15T10:30:00+02:00",
        "text": "/help topic",
        "isSystem": false,
        "uuid": null,
        "forward": null,
        "params": [],
        "viewedByOthers": false
    },
    "chat": {
        "id": 5,
        "dialogId": "chat5",
        "type": "chat",
        "name": "Support Chat"
    },
    "user": {
        "id": 1,
        "name": "John Smith",
        "bot": false,
        "type": "user"
    },
    "language": "en",
    "additionalMessages": []
}
```

---

## ONIMBOTV2REACTIONCHANGE {#onimbotv2reactionchange}

A reaction to the bot's message has been added or removed. The event is sent only if reactions are enabled for the bot — the `fields.isReactionsEnabled` field in [imbot.v2.Bot.register](../bots/bot-register.md) or [imbot.v2.Bot.update](../bots/bot-update.md).

#|
|| **Field** | **Type** | **Description** ||
|| **bot** | `object` | [Bot object](#bot-format) ||
|| **reaction** | `string` | Reaction code (e.g., `like`). The list of codes is in [imbot.v2.Chat.Message.Reaction.add](../messages/chat-message-reaction-add.md#reactions) ||
|| **action** | `string` | Action: `add` — reaction added, `delete` — removed ||
|| **message** | [`Message`](../../entities.md#message) | The message to which the reaction was added ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat ||
|| **user** | [`User`](../../entities.md#user) | The user who changed the reaction ||
|| **language** | `string` | Bitrix24 language ||
|| **additionalMessages** | `array` | Array of [`Message`](../../entities.md#message) objects: the original message if `message` is a reply to it. If it is not a reply, an empty array is returned, and in `webhook` mode the field is absent ||
|#

### Example Data

```json
{
    "bot": {
        "id": 456,
        "code": "support_bot",
        "type": "bot",
        "eventMode": "fetch"
    },
    "reaction": "like",
    "action": "add",
    "message": {
        "id": 789,
        "chatId": 5,
        "authorId": 456,
        "date": "2025-01-15T10:30:00+02:00",
        "text": "Hello! How can I help?",
        "isSystem": false,
        "uuid": null,
        "forward": null,
        "params": [],
        "viewedByOthers": true
    },
    "chat": {
        "id": 5,
        "dialogId": "chat5",
        "type": "chat",
        "name": "Support Chat"
    },
    "user": {
        "id": 1,
        "name": "John Smith",
        "bot": false,
        "type": "user"
    },
    "language": "en",
    "additionalMessages": []
}
```

## Continue Learning

- [API Change Log for imbot.v2](../../change-log.md)
- [Events imbot.v2](./index.md)
- [{#T}](./event-get.md)
- [{#T}](../../entities.md)
- [{#T}](../messages/chat-message-reaction-add.md)