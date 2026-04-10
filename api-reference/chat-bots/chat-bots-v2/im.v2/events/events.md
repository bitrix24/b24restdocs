# Event Formats im.v2

Description of all events that the user application receives via [im.v2.Event.get](./event-get.md).

The fields of the `message`, `chat`, and `user` objects are described in [{#T}](../../entities.md).

**Quick Navigation:** [ONIMV2MESSAGEADD](#onimv2messageadd) | [ONIMV2MESSAGEUPDATE](#onimv2messageupdate) | [ONIMV2MESSAGEDELETE](#onimv2messagedelete) | [ONIMV2REACTIONCHANGE](#onimv2reactionchange) | [ONIMV2JOINCHAT](#onimv2joinchat)

## Differences from Method Responses

In these events, the `chat` and `user` objects are returned in a shortened format:

- The `chat` object does not contain the `role` and `muteList` fields — these depend on the specific user and cannot be the same for all recipients.
- The `user` object does not contain the `network`, `botData`, and `avatarHr` fields.
- The online status fields (`idle`, `lastActivityDate`, `mobileLastDate`, `desktopLastDate`) are always set to `false`.

{% note info "" %}

This article describes the event format of the `im.v2.Event.get` method (polling/FETCH), so the `auth` field in the event data is not returned.

If using a webhook subscription for events, the webhook wrapper may contain an `auth` object with tokens.

{% endnote %}

---

## ONIMV2MESSAGEADD {#onimv2messageadd}

A new message in the chat that the subscribed user is part of.

#| 
|| **Field** | **Type** | **Description** ||
|| **message** | [`Message`](../../entities.md#message) | The sent message. The description of the object's fields — [Message](../../entities.md#message) ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat where the message was sent. The description of the object's fields — [Chat](../../entities.md#chat) ||
|| **user** | [`User`](../../entities.md#user) | The author of the message. The description of the object's fields — [User](../../entities.md#user) ||
|| **language** | `string` | The language of the account (e.g., `en`, `de`) ||
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
        "uuid": "",
        "forward": null,
        "params": {},
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
        "entityLink": {},
        "diskFolderId": 42,
        "permissions": {},
        "parentChatId": 0,
        "parentMessageId": 0,
        "isNew": false,
        "textFieldEnabled": "Y",
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
        "type": "employee"
    },
    "language": "en"
}
```

---

## ONIMV2MESSAGEUPDATE {#onimv2messageupdate}

A message in the chat has been edited.

#| 
|| **Field** | **Type** | **Description** ||
|| **message** | [`Message`](../../entities.md#message) | The updated message. The description of the object's fields — [Message](../../entities.md#message) ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat where the message was edited. The description of the object's fields — [Chat](../../entities.md#chat) ||
|| **user** | [`User`](../../entities.md#user) | The author of the message. The description of the object's fields — [User](../../entities.md#user) ||
|| **language** | `string` | The language of the account ||
|#

The data format is identical to [ONIMV2MESSAGEADD](#onimv2messageadd). The `message` field contains the updated text.

---

## ONIMV2MESSAGEDELETE {#onimv2messagedelete}

A message in the chat has been deleted.

#| 
|| **Field** | **Type** | **Description** ||
|| **messageId** | `integer` | ID of the deleted message ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat where the message was deleted. The description of the object's fields — [Chat](../../entities.md#chat) ||
|| **user** | [`User`](../../entities.md#user) | The author of the message. The description of the object's fields — [User](../../entities.md#user) ||
|| **language** | `string` | The language of the account ||
|#

---

## ONIMV2REACTIONCHANGE {#onimv2reactionchange}

A reaction to a message in the chat has been added or removed.

#| 
|| **Field** | **Type** | **Description** ||
|| **reaction** | `string` | Reaction code (e.g., `like`) ||
|| **action** | `string` | Action: `add` — reaction added, `delete` — removed ||
|| **message** | [`Message`](../../entities.md#message) | The message to which the reaction has changed. The description of the object's fields — [Message](../../entities.md#message) ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat. The description of the object's fields — [Chat](../../entities.md#chat) ||
|| **user** | [`User`](../../entities.md#user) | The user who changed the reaction. The description of the object's fields — [User](../../entities.md#user) ||
|| **language** | `string` | The language of the account ||
|#

---

## ONIMV2JOINCHAT {#onimv2joinchat}

A new participant has been added to the chat.

#| 
|| **Field** | **Type** | **Description** ||
|| **dialogId** | `string` | ID of the dialog (e.g., `chat5`) ||
|| **chat** | [`Chat`](../../entities.md#chat) | The chat to which the participant has been added. The description of the object's fields — [Chat](../../entities.md#chat) ||
|| **user** | [`User`](../../entities.md#user) | The added user. The description of the object's fields — [User](../../entities.md#user) ||
|| **language** | `string` | The language of the account ||
|#

## Continue Learning

- [API imbot.v2 Change Log](../../change-log.md)
- [{#T}](./event-get.md)
- [{#T}](./event-subscribe.md)
- [{#T}](../../entities.md)