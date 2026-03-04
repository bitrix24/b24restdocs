# Event for Deleting a Chatbot ONIMBOTDELETE

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can subscribe: a user of the application that registered the chatbot

The `ONIMBOTDELETE` event is triggered when a chatbot is deleted. This event operates solely within the context of the chatbot application.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONIMBOTDELETE",
    "event_handler_id": "439",
    "data": {
        "BOT_ID": "567",
        "BOT_CODE": "BOT"
    },
    "ts": "1772090363",
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
}
```

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../data-types.md) | Symbolic code of the event.

In this case — `ONIMBOTDELETE` ||
|| **event_handler_id**
[`integer`](../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../data-types.md) | Object containing data of the deleted chatbot.

The structure is described [below](#data) ||
|| **ts**
[`timestamp`](../../data-types.md) | Date and time the event was sent from the [event queue](../../events/index.md) ||
|| **auth**
[`object`](../../data-types.md) | Object containing authorization parameters of the user on behalf of whom the event was triggered.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

#|
|| **Parameter**
`type` | **Description** ||
|| **BOT_ID**
[`integer`](../../data-types.md) | Identifier of the deleted chatbot ||
|| **BOT_CODE**
[`string`](../../data-types.md) | Symbolic code of the deleted chatbot ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../_includes/auth-params-in-events.md) %}

## Continue Exploring

- [{#T}](../imbot-unregister.md)
- [{#T}](../chats/events/on-imbot-join-chat.md)
- [{#T}](../messages/events/on-imbot-message-add.md)
- [{#T}](../messages/events/on-imbot-message-update.md)
- [{#T}](../messages/events/on-imbot-message-delete.md)
- [{#T}](../commands/events/on-im-command-add.md)