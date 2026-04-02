# Event on Message Deletion ONIMBOTMESSAGEDELETE

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can subscribe: a user of the application that registered the chat bot

{% note warning "DEPRECATED" %}

The development of this event has been halted. Please use [ONIMBOTV2MESSAGEDELETE](../../../chat-bots-v2/imbot.v2/events/events.md#onimbotv2messagedelete).

{% endnote %}

The event `ONIMBOTMESSAGEDELETE` is triggered when a message is deleted in a dialogue with a chat bot. This event only works within the context of the chat bot application.

For bots with `TYPE=B/H` and `OPENLINE=N`, the parameter `EVENT_MESSAGE_DELETE` is ignored in the method [imbot.register](../../bots/imbot-register.md). To trigger the event for such bots, bind the handler `EVENT_MESSAGE_DELETE` using the method [imbot.update](../../bots/imbot-update.md).

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Event Triggering Rule

The event is triggered if the handler `EVENT_MESSAGE_DELETE` is bound and the message matches the bot type logic:
- `TYPE=B/H`:
    - personal chat with the bot: the event triggers regardless of mentions,
    - group chat: triggers only when the bot is mentioned,
- `TYPE=S` — the event triggers regardless of mentions,
- `TYPE=O` — the event triggers in open line chats where the bot is involved.

Without `EVENT_MESSAGE_DELETE`, the event will not trigger regardless of mentions.

## What the Handler Receives

Data is transmitted in the form of a POST request {.b24-info}

{% list tabs %}

- Personal chat with the bot

    ```json
    {
        "event": "ONIMBOTMESSAGEDELETE",
        "event_handler_id": "457",
        "data": {
            "BOT": {
                "571": {
                    "access_token": "9a0aa06900071b0000084400023bf0f107e7cd778b0dbccd0155cea",
                    "expires": "1772096154",
                    "expires_in": "3600",
                    "scope": "imbot",
                    "domain": "some-domain.bitrix24.com",
                    "server_endpoint": "https://oauth.bitrix.info/rest/",
                    "status": "F",
                    "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                    "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
                    "refresh_token": "9a0aa06900071b000844000023bf0f107e7cd778b0dbccd0155cea",
                    "user_id": "571",
                    "client_id": "a7eff906dd1d950269258a599214f69e",
                    "application_token": "831c76b092f9f135d9b6b36c3a720757",
                    "AUTH": {
                        "access_token": "9a0aa06900071b0000084400023bf0f107e7cd778b0dbccd0155cea",
                        "expires": "1772096154",
                        "expires_in": "3600",
                        "scope": "imbot",
                        "domain": "some-domain.bitrix24.com",
                        "server_endpoint": "https://oauth.bitrix.info/rest/",
                        "status": "F",
                        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                        "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
                        "refresh_token": "9a0aa06900071b000844000023bf0f107e7cd778b0dbccd0155cea",
                        "user_id": "571",
                        "client_id": "a7eff906dd1d950269258a599214f69e",
                        "application_token": "831c76b092f9f135d9b6b36c3a720757"
                    },
                    "BOT_ID": "571",
                    "BOT_CODE": "BOT"
                }
            },
            "PARAMS": {
                "ID": "84525",
                "CHAT_ID": "1453",
                "AUTHOR_ID": "27",
                "MESSAGE": "Message from 02/26/2026 09:27:30 has been deleted",
                "MESSAGE_TYPE": "P",
                "CHAT_AUTHOR_ID": "571",
                "CHAT_ENTITY_ID": "",
                "CHAT_ENTITY_DATA_1": "",
                "CHAT_ENTITY_DATA_2": "",
                "CHAT_ENTITY_DATA_3": "",
                "FROM_USER_ID": "27",
                "TO_USER_ID": "571",
                "DIALOG_ID": "27",
                "MESSAGE_ID": "84525",
                "CHAT_TYPE": "P",
                "LANGUAGE": "de"
            },
            "USER": {
                "ID": "27",
                "NAME": "Svetlana Ivanova",
                "FIRST_NAME": "Svetlana",
                "LAST_NAME": "Ivanova",
                "WORK_POSITION": "",
                "GENDER": "F",
                "IS_BOT": "N",
                "IS_CONNECTOR": "N",
                "IS_NETWORK": "N",
                "IS_EXTRANET": "N"
            }
        },
        "ts": "1772092554",
        "auth": {
            "access_token": "9a0aa06900071b00084400001b0000781c0546d93491d81d",
            "expires": "1772096154",
            "expires_in": "3600",
            "scope": "imbot",
            "domain": "some-domain.bitrix24.com",
            "server_endpoint": "https://oauth.bitrix.info/rest/",
            "status": "F",
            "client_endpoint": "https://some-domain.bitrix24.com/rest/",
            "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
            "user_id": "27",
            "refresh_token": "8a89c769000071b00084400001b000c1b724e4652017b0ea7051ffb8e",
            "application_token": "831c76b092f9f135d9b6b36c3a720757"
        }
    }
    ```

- Group chat

    ```json
    {
        "event": "ONIMBOTMESSAGEDELETE",
        "event_handler_id": "457",
        "data": {
            "BOT": {
                "571": {
                    "access_token": "9a0aa0690000071b000008440001b000781c0546d82fee9e8c",
                    "expires": "1772100933",
                    "expires_in": "3600",
                    "scope": "imbot",
                    "domain": "some-domain.bitrix24.com",
                    "server_endpoint": "https://oauth.bitrix.info/rest/",
                    "status": "F",
                    "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                    "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
                    "refresh_token": "9a0aa0690071b0000084400001b000781c0546d82fee9e8c",
                    "user_id": "571",
                    "client_id": "a7eff906dd1d950269258a599214f69e",
                    "application_token": "831c76b092f9f135d9b6b36c3a720757",
                    "AUTH": {
                        "access_token": "9a0aa0690000071b000008440001b000781c0546d82fee9e8c",
                        "expires": "1772100933",
                        "expires_in": "3600",
                        "scope": "imbot",
                        "domain": "some-domain.bitrix24.com",
                        "server_endpoint": "https://oauth.bitrix.info/rest/",
                        "status": "F",
                        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                        "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
                        "refresh_token": "9a0aa0690071b0000084400001b000781c0546d82fee9e8c",
                        "user_id": "571",
                        "client_id": "a7eff906dd1d950269258a599214f69e",
                        "application_token": "831c76b092f9f135d9b6b36c3a720757"
                    },
                    "BOT_ID": "571",
                    "BOT_CODE": "BOT"
                }
            },
            "PARAMS": {
                "ID": "84537",
                "CHAT_ID": "1157",
                "AUTHOR_ID": "27",
                "MESSAGE": "Message from 02/26/2026 11:14:02 has been deleted",
                "MESSAGE_TYPE": "C",
                "CHAT_AUTHOR_ID": "27",
                "CHAT_ENTITY_TYPE": "THREAD",
                "CHAT_ENTITY_ID": "",
                "CHAT_ENTITY_DATA_1": "",
                "CHAT_ENTITY_DATA_2": "",
                "CHAT_ENTITY_DATA_3": "",
                "DIALOG_ID": "chat1157",
                "MESSAGE_ID": "84537",
                "CHAT_TYPE": "C",
                "LANGUAGE": "de"
            }
        },
        "ts": "1772097333",
        "auth": {
            "access_token": "451da069000071b00084400001b00075b8bc870e6dbc3",
            "expires": "1772100933",
            "expires_in": "3600",
            "scope": "imbot",
            "domain": "some-domain.bitrix24.com",
            "server_endpoint": "https://oauth.bitrix.info/rest/",
            "status": "F",
            "client_endpoint": "https://some-domain.bitrix24.com/rest/",
            "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
            "user_id": "27",
            "refresh_token": "359cc76900071b0000084400001b00071a54cd7aad70b5d2fa",
            "application_token": "831c76b092f9f135d9b6b36c3a720757"
        }
    }
    ```

{% endlist %}

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../../data-types.md) | Symbolic event code.

In this case — `ONIMBOTMESSAGEDELETE` ||
|| **event_handler_id**
[`integer`](../../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../../data-types.md) | Object containing event data.

The structure is described [below](#data) ||
|| **ts**
[`timestamp`](../../../../data-types.md) | Date and time the event was sent from the [event queue](../../../../events/index.md) ||
|| **auth**
[`object`](../../../../data-types.md) | Object containing authorization parameters of the user on behalf of whom the event was triggered.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

#|
|| **Parameter**
`type` | **Description** ||
|| **BOT**
[`object`](../../../../data-types.md) | Set of authorization parameters for the bots to which the message is intended. The key of the object is the bot identifier `BOT_ID`.

The structure is described [below](#bot) ||
|| **PARAMS**
[`object`](../../../../data-types.md) | Parameters of the deleted message.

The structure is described [below](#params) ||
|| **USER**
[`object`](../../../../data-types.md) | Data of the author of the deleted message. It may be an empty object if `FROM_USER_ID = 0`.

The structure is described [below](#user) ||
|#

### Parameter BOT {#bot}

#|
|| **Parameter**
`type` | **Description** ||
|| **{BOT_ID}**
[`object`](../../../../data-types.md) | Data object of a specific bot. The key corresponds to the bot identifier, for example `571`.

The structure is described [below](#bot-item) ||
|#

#### Element /{BOT_ID/} {#bot-item}

#|
|| **Parameter**
`type` | **Description** ||
|| **access_token**
[`string`](../../../../data-types.md) | OAuth authorization token for the bot ||
|| **expires**
[`timestamp`](../../../../data-types.md) | Moment the token expires ||
|| **expires_in**
[`integer`](../../../../data-types.md) | Token lifetime in seconds ||
|| **scope**
[`string`](../../../../data-types.md) | Scope within which the event occurred ||
|| **domain**
[`string`](../../../../data-types.md) | Bitrix24 address where the event occurred ||
|| **server_endpoint**
[`string`](../../../../data-types.md) | Address of the OAuth server for REST requests ||
|| **status**
[`string`](../../../../data-types.md) | Status indicator of the application on the account ||
|| **client_endpoint**
[`string`](../../../../data-types.md) | General path for calling REST API methods on the account where the event occurred ||
|| **member_id**
[`string`](../../../../data-types.md) | Unique identifier of Bitrix24 ||
|| **refresh_token**
[`string`](../../../../data-types.md) | OAuth token for renewing the bot's authorization ||
|| **user_id**
[`integer`](../../../../data-types.md) | Identifier of the bot user ||
|| **client_id**
[`string`](../../../../data-types.md) | Identifier of the application issued upon registration ||
|| **application_token**
[`string`](../../../../data-types.md) | Application token ||
|| **AUTH**
[`object`](../../../../data-types.md) | Authorization parameters of the bot in `auth` format.

The structure is described [below](#auth) ||
|| **BOT_ID**
[`integer`](../../../../data-types.md) | Identifier of the bot ||
|| **BOT_CODE**
[`string`](../../../../data-types.md) | Symbolic code of the bot ||
|#

### Parameter PARAMS {#params}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`](../../../../data-types.md) | Identifier of the message in the chat table ||
|| **CHAT_ID**
[`integer`](../../../../data-types.md) | Identifier of the chat.

For group chat and open line chat, the parameter is always passed. For personal dialogue, the parameter may be absent ||
|| **AUTHOR_ID**
[`integer`](../../../../data-types.md) | Identifier of the author of the deleted message ||
|| **MESSAGE**
[`string`](../../../../data-types.md) | Text of the system notification about the deleted message ||
|| **MESSAGE_TYPE**
[`string`](../../../../data-types.md) | Type of message.

Possible values:
- `P` — private, personal chat
- `C` — group chat
- `O` — open chat
- `L` — open line
- `S` — system/notify
- `N` — channel
- `J` — open channel
- `T` — comment thread
- `A` — copilot chat
- `B` — collab
- `X` — external chat ||
|| **CHAT_AUTHOR_ID**
[`integer`](../../../../data-types.md) | Identifier of the chat owner ||
|| **CHAT_ENTITY_TYPE**
[`string`](../../../../data-types.md) | Type of the object to which the chat is linked ||
|| **CHAT_ENTITY_ID**
[`string`](../../../../data-types.md) | Identifier of the object to which the chat is linked ||
|| **CHAT_ENTITY_DATA_1**
[`string`](../../../../data-types.md) | Additional data of the chat object — field 1 ||
|| **CHAT_ENTITY_DATA_2**
[`string`](../../../../data-types.md) | Additional data of the chat object — field 2 ||
|| **CHAT_ENTITY_DATA_3**
[`string`](../../../../data-types.md) | Additional data of the chat object — field 3 ||
|| **DIALOG_ID**
[`string`](../../../../data-types.md) | Identifier of the dialogue ||
|| **MESSAGE_ID**
[`integer`](../../../../data-types.md) | Identifier of the deleted message. Added in the event handler from the second argument of the event ||
|| **CHAT_TYPE**
[`string`](../../../../data-types.md) | Type of chat. 

Possible values:
- `P` — private, personal chat
- `C` — group chat
- `O` — open chat
- `L` — open line
- `S` — system/notify
- `N` — channel
- `J` — open channel
- `T` — comment thread
- `A` — copilot chat
- `B` — collab
- `X` — external chat ||
|| **LANGUAGE**
[`string`](../../../../data-types.md) | Default language of Bitrix24 ||
|#

### Parameter USER {#user}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`](../../../../data-types.md) | Identifier of the user ||
|| **NAME**
[`string`](../../../../data-types.md) | Full name of the user ||
|| **FIRST_NAME**
[`string`](../../../../data-types.md) | First name of the user ||
|| **LAST_NAME**
[`string`](../../../../data-types.md) | Last name of the user ||
|| **WORK_POSITION**
[`string`](../../../../data-types.md) | User's position ||
|| **GENDER**
[`string`](../../../../data-types.md) | User's gender: `M` or `F` ||
|| **IS_BOT**
[`string`](../../../../data-types.md) | Bot user indicator: `Y` or `N` ||
|| **IS_CONNECTOR**
[`string`](../../../../data-types.md) | Connector user indicator: `Y` or `N` ||
|| **IS_NETWORK**
[`string`](../../../../data-types.md) | External network user indicator: `Y` or `N` ||
|| **IS_EXTRANET**
[`string`](../../../../data-types.md) | Extranet user indicator: `Y` or `N` ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [Event on Message Sent ONIMBOTMESSAGEADD](./on-imbot-message-add.md)
- [Event on Message Update ONIMBOTMESSAGEUPDATE](./on-imbot-message-update.md)
- [Event for Deleting Chat-Bot ONIMBOTDELETE](../../events/on-imbot-delete.md)
- [Delete Message imbot.message.delete](../imbot-message-delete.md)
- [Create a Chat-Bot imbot.register](../../bots/imbot-register.md)
