# Event Formats for imbot.v2

Description of all events that the bot receives via [imbot.v2.Event.get](./event-get.md) (FETCH mode) or through webhook.

The fields of the `message`, `chat`, and `user` objects are described in [Objects and Fields of Chatbots 2.0](../../entities.md).

## Which Events to Process First

The minimum set of events for a working bot:

- [ONIMBOTV2MESSAGEADD](#onimbotv2messageadd) — incoming messages from users
- [ONIMBOTV2COMMANDADD](#onimbotv2commandadd) — invocation of slash commands
- [ONIMBOTV2JOINCHAT](#onimbotv2joinchat) — bot added to a chat (usually sends a greeting)
- [ONIMBOTV2DELETE](#onimbotv2delete) — bot removal (resource cleanup)

Additionally, based on the scenario:

- [ONIMBOTV2MESSAGEUPDATE](#onimbotv2messageupdate) and [ONIMBOTV2MESSAGEDELETE](#onimbotv2messagedelete) — if you support editing/deletion
- [ONIMBOTV2REACTIONCHANGE](#onimbotv2reactionchange) — if you track reactions
- [ONIMBOTV2CONTEXTGET](#onimbotv2contextget) — if you use the input context of the dialogue

## Bot Object Format {#bot-format}

The content of the `bot` field depends on the event delivery mode.

- **FETCH** (`imbot.v2.Event.get`) — full bot object, as in the response from [imbot.v2.Bot.get](../bots/bot-get.md)
- **Webhook** — simplified object `{id, code, auth}`, where `auth` contains the OAuth token for callbacks

Each webhook call contains data for **one** bot. If the application has registered multiple bots and the event pertains to several of them, the webhook is called separately for each.

Example of the `bot` object in webhook mode:

```json
{
    "bot": {
        "id": 456,
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
            "user_id": "27",
            "application_token": "831c76b092f9f135d9b6b36c3a720757"
        }
    },
    "message": {},
    "chat": {},
    "user": {}
}
```

### Auth Parameter {#auth}

{% include notitle [Table with keys in the auth array](../../../../../_includes/auth-params-in-events.md) %}

{% note info "" %}

The `auth` field depends on the event delivery mode:

- in **Webhook** mode (`bot` is sent to the specified handler), the `auth` field is present and contains tokens for callback authorization
- in **FETCH** mode (`imbot.v2.Event.get`), the `auth` field in the `bot` object is not returned, as tokens are not required

{% endnote %}

## Data Types in Webhook Mode

Webhook events are delivered through the Bitrix24 event system, which serializes data via `http_build_query`. Because of this, **all scalar values in webhook mode are transmitted as strings**.

#|
|| **Type** | **Value in FETCH** | **Value in Webhook** ||
|| `integer` | `789` | `"789"` ||
|| `boolean` | `true` / `false` | `"1"` / `"0"` ||
|| `string|false` | `false` | `"0"` ||
|| `null` | `null` | `""` ||
|#

When processing webhook events, it is recommended to cast types explicitly: `(int)$data['messageId']`, `$data['user']['active'] !== '0'`. In FETCH mode, types correspond to the documentation.

---

## ONIMBOTV2MESSAGEADD {#onimbotv2messageadd}

A new message addressed to the bot. This occurs when a user sends a message in a chat that the bot is part of.

#|
|| **Field** | **Type** | **Description** ||
|| **bot** | `object` | [Bot object](#bot-format) ||
|| **message** | [`Message`](../../entities.md#message) | The sent message ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat in which the message was sent ||
|| **user** | [`User`](../../entities.md#user) | The author of the message ||
|| **language** | `string` | Portal language (e.g., `en`, `de`) ||
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
        "appId": "custom123abc",
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
        "date": "2025-01-15T10:30:00+01:00",
        "text": "Hello bot!",
        "isSystem": false,
        "uuid": "",
        "forward": null,
        "params": {},
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
        "lastActivityDate": "2025-01-15T10:29:00+01:00",
        "absent": false,
        "departments": [1],
        "phones": false,
        "type": "employee"
    },
    "language": "en"
}
```

---

## ONIMBOTV2MESSAGEUPDATE {#onimbotv2messageupdate}

A message in the bot's chat has been edited.

#|
|| **Field** | **Type** | **Description** ||
|| **bot** | `object` | [Bot object](#bot-format) ||
|| **message** | [`Message`](../../entities.md#message) | The updated message ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat in which the message was edited ||
|| **user** | [`User`](../../entities.md#user) | The author of the message ||
|| **language** | `string` | Portal language ||
|#

The data format is identical to [ONIMBOTV2MESSAGEADD](#onimbotv2messageadd). The `message` field contains the updated text.

---

## ONIMBOTV2MESSAGEDELETE {#onimbotv2messagedelete}

A message in the bot's chat has been deleted.

#|
|| **Field** | **Type** | **Description** ||
|| **bot** | `object` | [Bot object](#bot-format) ||
|| **messageId** | `integer` | ID of the deleted message ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat in which the message was deleted ||
|| **user** | [`User`](../../entities.md#user) | The author of the deleted message ||
|| **language** | `string` | Portal language ||
|#

---

## ONIMBOTV2JOINCHAT {#onimbotv2joinchat}

The bot has been added to a chat or received an invitation.

#|
|| **Field** | **Type** | **Description** ||
|| **bot** | `object` | [Bot object](#bot-format) ||
|| **dialogId** | `string` | ID of the dialog (e.g., `chat5`) ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat to which the bot was added ||
|| **user** | [`User`](../../entities.md#user) | The user who added the bot ||
|| **language** | `string` | Portal language ||
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
        "appId": "custom123abc",
        "eventMode": "fetch",
        "countMessage": 150,
        "countCommand": 3,
        "countChat": 12,
        "countUser": 45
    },
    "dialogId": "chat5",
    "chat": {
        "id": 5,
        "dialogId": "chat5",
        "type": "chat",
        "name": "Project Chat",
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
        "lastActivityDate": "2025-01-15T10:29:00+01:00",
        "absent": false,
        "departments": [1],
        "phones": false,
        "type": "employee"
    },
    "language": "en"
}
```

---

## ONIMBOTV2DELETE {#onimbotv2delete}

The bot has been removed from the system. This is the last event the bot will receive.

#|
|| **Field** | **Type** | **Description** ||
|| **bot** | `object` | [Bot object](#bot-format) ||
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
        "appId": "custom123abc",
        "eventMode": "fetch",
        "countMessage": 150,
        "countCommand": 3,
        "countChat": 12,
        "countUser": 45
    }
}
```

---

## ONIMBOTV2CONTEXTGET {#onimbotv2contextget}

The user opened a dialogue with the bot, passing arbitrary context data. The context is set by the calling party — for example, when navigating via a link with the `botContextData` parameter.

#|
|| **Field** | **Type** | **Description** ||
|| **bot** | `object` | [Bot object](#bot-format) ||
|| **dialogId** | `string` | ID of the dialog (e.g., `chat5`) ||
|| **context** | `object` | Arbitrary data passed when opening the dialog ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat ||
|| **user** | [`User`](../../entities.md#user) | The user who opened the dialog ||
|| **language** | `string` | Portal language ||
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
        "appId": "custom123abc",
        "eventMode": "fetch",
        "countMessage": 150,
        "countCommand": 3,
        "countChat": 12,
        "countUser": 45
    },
    "dialogId": "chat5",
    "context": {
        "entityId": 164,
        "entityType": "task",
        "source": "link"
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
        "lastActivityDate": "2025-01-15T10:29:00+01:00",
        "absent": false,
        "departments": [1],
        "phones": false,
        "type": "employee"
    },
    "language": "en"
}
```

---

## ONIMBOTV2COMMANDADD {#onimbotv2commandadd}

The user invoked a slash command of the bot.

#|
|| **Field** | **Type** | **Description** ||
|| **bot** | `object` | [Bot object](#bot-format) ||
|| **command** | `object` | Command data. Field descriptions — [below](#command-object) ||
|| **message** | [`Message`](../../entities.md#message) | The message with the command ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat from which the command was invoked ||
|| **user** | [`User`](../../entities.md#user) | The user who invoked the command ||
|| **language** | `string` | Portal language ||
|#

### Command Object {#command-object}

#|
|| **Field** | **Type** | **Description** ||
|| **id** | `integer` | ID of the registered command ||
|| **command** | `string` | Invoked command (e.g., `/help`) ||
|| **params** | `string` | Command parameters — text following the command ||
|| **context** | `string` | Invocation context: `textarea` — entered manually, `keyboard` — button pressed, `menu` — selected from context menu ||
|#

{% note info "" %}

If a single message contains multiple slash commands, an event is generated separately for each command.

{% endnote %}

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
        "appId": "custom123abc",
        "eventMode": "fetch",
        "countMessage": 150,
        "countCommand": 3,
        "countChat": 12,
        "countUser": 45
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
        "date": "2025-01-15T10:30:00+01:00",
        "text": "/help topic",
        "isSystem": false,
        "uuid": "",
        "forward": null,
        "params": {},
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
        "lastActivityDate": "2025-01-15T10:29:00+01:00",
        "absent": false,
        "departments": [1],
        "phones": false,
        "type": "employee"
    },
    "language": "en"
}
```

---

## ONIMBOTV2REACTIONCHANGE {#onimbotv2reactionchange}

A reaction to the bot's message has been added or removed.

#|
|| **Field** | **Type** | **Description** ||
|| **bot** | `object` | [Bot object](#bot-format) ||
|| **reaction** | `string` | Reaction code (e.g., `like`). A list of codes can be found in [imbot.v2.Chat.Message.Reaction.add](../messages/chat-message-reaction-add.md#reactions) ||
|| **action** | `string` | Action: `add` — reaction added, `delete` — removed ||
|| **message** | [`Message`](../../entities.md#message) | The message to which the reaction was added ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat ||
|| **user** | [`User`](../../entities.md#user) | The user who changed the reaction ||
|| **language** | `string` | Portal language ||
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
        "appId": "custom123abc",
        "eventMode": "fetch",
        "countMessage": 150,
        "countCommand": 3,
        "countChat": 12,
        "countUser": 45
    },
    "reaction": "like",
    "action": "add",
    "message": {
        "id": 789,
        "chatId": 5,
        "authorId": 456,
        "date": "2025-01-15T10:30:00+01:00",
        "text": "Hello! How can I help?",
        "isSystem": false,
        "uuid": "",
        "forward": null,
        "params": {},
        "viewedByOthers": true
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
        "lastActivityDate": "2025-01-15T10:29:00+01:00",
        "absent": false,
        "departments": [1],
        "phones": false,
        "type": "employee"
    },
    "language": "en"
}
```

## Continue Learning

- [Get Bot Events imbot.v2.Event.get](./event-get.md)
- [Objects and Fields of Chatbots 2.0](../../entities.md)
- [Add Reaction to Message imbot.v2.Chat.Message.Reaction.add](../messages/chat-message-reaction-add.md)