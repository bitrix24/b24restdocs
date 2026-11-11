# Event Formats im.v2

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`im`](../../../../scopes/permissions.md)
>
> Who can subscribe: an authorized user

This document describes all events that an application or a user receives via [im.v2.Event.get](./event-get.md).

Bitrix24 records events only after [im.v2.Event.subscribe](./event-subscribe.md) is called on behalf of the user, and delivers them only in polling mode — by calls to [im.v2.Event.get](./event-get.md). The subscription and polling procedure is described in the overview [Working with Chat](../index.md).

Each event is an element of the `result.events` array with the `eventId`, `type`, `date`, and `data` fields. The tables and examples below describe the contents of `data`; the other fields are described on the [im.v2.Event.get](./event-get.md) page. The fields of the `message`, `chat`, and `user` objects are described in [{#T}](../../entities.md).

> Quick navigation: [All Events](#all-events)

## Overview of Events {#all-events}

#|
|| **Event** | **When it arrives** ||
|| [ONIMV2MESSAGEADD](#onimv2messageadd) | A new message in a chat where the subscribed user is a member ||
|| [ONIMV2MESSAGEUPDATE](#onimv2messageupdate) | A message has been edited ||
|| [ONIMV2MESSAGEDELETE](#onimv2messagedelete) | A message has been deleted ||
|| [ONIMV2REACTIONCHANGE](#onimv2reactionchange) | A reaction to a message has been added or removed ||
|| [ONIMV2JOINCHAT](#onimv2joinchat) | A new participant has been added to a chat ||
|#

## Differences from Method Responses

In event data, the `chat` and `user` objects are shortened:

- `chat` — without the `role` and `muteList` fields: they depend on the specific user and cannot be the same for all recipients
- `user` — without the `network`, `botData`, and `avatarHr` fields, and the online status fields `idle`, `lastActivityDate`, `mobileLastDate`, `desktopLastDate` are always `false`

{% note info "" %}

There is no `auth` field in the event data. `im.v2` events arrive in the [im.v2.Event.get](./event-get.md) response rather than in the application handler, so authorization is passed in the request itself.

{% endnote %}

---

## ONIMV2MESSAGEADD {#onimv2messageadd}

A new message in a chat that the subscribed user is part of, including a message sent by the user.

#| 
|| **Field** | **Type** | **Description** ||
|| **message** | [`Message`](../../entities.md#message) | The sent message. Field descriptions for the object — [Message](../../entities.md#message) ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat where the message was sent. Field descriptions for the object — [Chat](../../entities.md#chat) ||
|| **user** | [`User`](../../entities.md#user) | The author of the message. Field descriptions for the object — [User](../../entities.md#user) ||
|| **language** | `string` | The language of Bitrix24 (e.g., `en`, `de`) ||
|| **additionalMessages** | `array` | Array of [`Message`](../../entities.md#message) objects. If `message` is a reply to another message, the array contains the original message; otherwise, the array is empty. If the original message has been deleted, it comes with a deletion notice as its text ||
|#

### Example Data

```json
{
    "message": {
        "id": 5012,
        "chatId": 5,
        "authorId": 1,
        "date": "2025-01-15T10:30:00+02:00",
        "text": "Hello everyone!",
        "isSystem": false,
        "uuid": null,
        "forward": null,
        "params": [],
        "viewedByOthers": false
    },
    "chat": {
        "id": 5,
        "dialogId": "chat5",
        "name": "Project Chat",
        "type": "chat",
        "messageType": "C",
        "owner": 1,
        "color": "#ab7761",
        "avatar": "",
        "description": "",
        "extranet": false,
        "containsCollaber": false,
        "entityType": "",
        "entityId": "",
        "entityData1": "",
        "entityData2": "",
        "entityData3": "",
        "entityLink": {
            "type": "",
            "url": "",
            "id": ""
        },
        "diskFolderId": 42,
        "permissions": {
            "manageUsersAdd": "member",
            "manageUsersDelete": "manager",
            "manageUi": "member",
            "manageSettings": "owner",
            "manageMessages": "member",
            "manageMessagesAutoDelete": "manager",
            "manageGuestInvites": "manager",
            "manageDelete": "member",
            "canPost": "member"
        },
        "parentChatId": 0,
        "parentMessageId": 0,
        "isNew": false,
        "textFieldEnabled": true,
        "backgroundId": null
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
        "mobileLastDate": false,
        "desktopLastDate": false,
        "absent": false,
        "departments": [1],
        "phones": false,
        "website": "",
        "email": "john@example.com",
        "type": "user"
    },
    "language": "en",
    "additionalMessages": []
}
```

---

## ONIMV2MESSAGEUPDATE {#onimv2messageupdate}

A message in the chat has been edited. If the message is updated with the [im.message.update](../../../../chats/messages/im-message-update.md) method, the event is not sent.

#| 
|| **Field** | **Type** | **Description** ||
|| **message** | [`Message`](../../entities.md#message) | The updated message. Field descriptions for the object — [Message](../../entities.md#message) ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat where the message was edited. Field descriptions for the object — [Chat](../../entities.md#chat) ||
|| **user** | [`User`](../../entities.md#user) | The author of the message. Field descriptions for the object — [User](../../entities.md#user) ||
|| **language** | `string` | The language of Bitrix24 ||
|| **additionalMessages** | `array` | Array of [`Message`](../../entities.md#message) objects. If `message` is a reply to another message, the array contains the original message; otherwise, the array is empty. If the original message has been deleted, it comes with a deletion notice as its text ||
|#

The data has the same structure as in [ONIMV2MESSAGEADD](#onimv2messageadd), except that `message` contains the message with the updated text.

---

## ONIMV2MESSAGEDELETE {#onimv2messagedelete}

A message in the chat has been deleted.

#| 
|| **Field** | **Type** | **Description** ||
|| **messageId** | `integer` | ID of the deleted message ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat where the message was deleted. Field descriptions for the object — [Chat](../../entities.md#chat) ||
|| **user** | [`User`](../../entities.md#user) | The author of the message. Field descriptions for the object — [User](../../entities.md#user) ||
|| **language** | `string` | The language of Bitrix24 ||
|#

### Example Data

The `chat` and `user` objects here and in the examples below are shortened. The [ONIMV2MESSAGEADD](#onimv2messageadd) example shows the most fields; all fields are described on the [{#T}](../../entities.md) page.

```json
{
    "messageId": 5012,
    "chat": {
        "id": 5,
        "dialogId": "chat5",
        "name": "Project Chat",
        "type": "chat"
    },
    "user": {
        "id": 1,
        "name": "John Smith",
        "type": "user"
    },
    "language": "en"
}
```

---

## ONIMV2REACTIONCHANGE {#onimv2reactionchange}

A reaction to a message in the chat has been added or removed.

#| 
|| **Field** | **Type** | **Description** ||
|| **reaction** | `string` | Reaction code (e.g., `like`) ||
|| **action** | `string` | Action with the reaction. In `im.v2` events, it comes as an empty string both when a reaction is added and when it is removed: the two cannot be distinguished by this field ||
|| **message** | [`Message`](../../entities.md#message) | The message to which a reaction was added or from which it was removed. Field descriptions for the object — [Message](../../entities.md#message) ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat. Field descriptions for the object — [Chat](../../entities.md#chat) ||
|| **user** | [`User`](../../entities.md#user) | The user who changed the reaction. Field descriptions for the object — [User](../../entities.md#user) ||
|| **language** | `string` | The language of Bitrix24 ||
|| **additionalMessages** | `array` | Array of [`Message`](../../entities.md#message) objects. If `message` is a reply to another message, the array contains the original message; otherwise, the array is empty. If the original message has been deleted, it comes with a deletion notice as its text ||
|#

### Example Data

```json
{
    "reaction": "like",
    "action": "",
    "message": {
        "id": 5012,
        "chatId": 5,
        "authorId": 1,
        "date": "2025-01-15T10:30:00+03:00",
        "text": "Hello everyone!",
        "isSystem": false,
        "uuid": null,
        "forward": null,
        "params": [],
        "viewedByOthers": false
    },
    "chat": {
        "id": 5,
        "dialogId": "chat5",
        "name": "Project Chat",
        "type": "chat"
    },
    "user": {
        "id": 2,
        "name": "Jane Doe",
        "type": "user"
    },
    "language": "en",
    "additionalMessages": []
}
```

---

## ONIMV2JOINCHAT {#onimv2joinchat}

A new participant has been added to the chat.

#| 
|| **Field** | **Type** | **Description** ||
|| **dialogId** | `string` | Dialog ID. For a group chat — `chat` followed by the chat ID, for example `chat5`. For a one-on-one chat — the ID of the other participant ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat to which the participant has been added. Field descriptions for the object — [Chat](../../entities.md#chat) ||
|| **user** | [`User`](../../entities.md#user) | The added user. Field descriptions for the object — [User](../../entities.md#user) ||
|| **language** | `string` | The language of Bitrix24 ||
|#

### Example Data

```json
{
    "dialogId": "chat5",
    "chat": {
        "id": 5,
        "dialogId": "chat5",
        "name": "Project Chat",
        "type": "chat"
    },
    "user": {
        "id": 3,
        "name": "Alex Brown",
        "type": "user"
    },
    "language": "en"
}
```

## Continue Learning

- [API Change Log imbot.v2](../../change-log.md)
- [im.v2 Events](./index.md)
- [{#T}](./event-get.md)
- [{#T}](./event-subscribe.md)
- [{#T}](../../entities.md)