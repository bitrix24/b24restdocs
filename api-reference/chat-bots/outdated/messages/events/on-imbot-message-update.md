# Event on Message Update ONIMBOTMESSAGEUPDATE

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can subscribe: a user of the application that registered the chat bot

{% note warning "DEPRECATED" %}

Development of this event has been halted. Please use [ONIMBOTV2MESSAGEUPDATE](../../../chat-bots-v2/imbot.v2/events/events.md#onimbotv2messageupdate).

{% endnote %}

The `ONIMBOTMESSAGEUPDATE` event is triggered when a message is updated in a chat with the chat bot. This event only works within the context of the chat bot application.

For bots with `TYPE=B/H` and `OPENLINE=N`, the `EVENT_MESSAGE_UPDATE` parameter is ignored in the [imbot.register](../../bots/imbot-register.md) method. To trigger the event for such bots, bind the `EVENT_MESSAGE_UPDATE` handler using the [imbot.update](../../bots/imbot-update.md) method.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Event Triggering Rule

The event is triggered if the `EVENT_MESSAGE_UPDATE` handler is bound and the message complies with the bot type logic:
- `TYPE=B/H`:
    - personal chat with the bot: the event triggers regardless of mentions,
    - group chat: triggers only when the bot is mentioned,
- `TYPE=S` — the event triggers regardless of mentions,
- `TYPE=O` — the event triggers in open line chats where the bot is involved.

Without `EVENT_MESSAGE_UPDATE`, the event will not trigger regardless of mentions.

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

{% list tabs %}

- Personal chat with the bot

    ```json
    {
        "event": "ONIMBOTMESSAGEUPDATE",
        "event_handler_id": "455",
        "data": {
            "BOT": {
                "571": {
                    "access_token": "880aa0690000071b000008440000023bf0f107a4af72f200b757ece",
                    "expires": "1772096136",
                    "expires_in": "3600",
                    "scope": "imbot",
                    "domain": "some-domain.bitrix24.com",
                    "server_endpoint": "https://oauth.bitrix.info/rest/",
                    "status": "F",
                    "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                    "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
                    "refresh_token": "7889c7690000071b000008440000023bf0f107ad212748fb2268",
                    "user_id": "571",
                    "client_id": "a7eff906dd1d950269258a599214f69e",
                    "application_token": "831c76b092f9f135d9b6b36c3a720757",
                    "AUTH": {
                        "access_token": "880aa0690000071b000008440000023bf0f107a4af72f200b757ece",
                        "expires": "1772096136",
                        "expires_in": "3600",
                        "scope": "imbot",
                        "domain": "some-domain.bitrix24.com",
                        "server_endpoint": "https://oauth.bitrix.info/rest/",
                        "status": "F",
                        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                        "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
                        "refresh_token": "7889c7690000071b000008440000023bf0f107ad212748fb2268",
                        "user_id": "571",
                        "client_id": "a7eff906dd1d950269258a599214f69e",
                        "application_token": "831c76b092f9f135d9b6b36c3a720757"
                    },
                    "BOT_ID": "571",
                    "BOT_CODE": "BOT"
                }
            },
            "PARAMS": {
                "ID": "84531",
                "CHAT_ID": "1453",
                "AUTHOR_ID": "27",
                "MESSAGE": "How to add an observer to the task?",
                "MESSAGE_TYPE": "P",
                "CHAT_AUTHOR_ID": "571",
                "CHAT_ENTITY_ID": "",
                "CHAT_ENTITY_DATA_1": "",
                "CHAT_ENTITY_DATA_2": "",
                "CHAT_ENTITY_DATA_3": "",
                "FROM_USER_ID": "27",
                "TO_USER_ID": "571",
                "CHAT_USER_COUNT": "2",
                "PLATFORM_CONTEXT": "web",
                "DIALOG_ID": "27",
                "MESSAGE_ID": "84531",
                "CHAT_TYPE": "P",
                "LANGUAGE": "en"
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
        "ts": "1772092536",
        "auth": {
            "access_token": "880aa06900071b00084400001b0007dff360d21523410a45b4f7c12",
            "expires": "1772096136",
            "expires_in": "3600",
            "scope": "imbot",
            "domain": "some-domain.bitrix24.com",
            "server_endpoint": "https://oauth.bitrix.info/rest/",
            "status": "F",
            "client_endpoint": "https://some-domain.bitrix24.com/rest/",
            "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
            "user_id": "27",
            "refresh_token": "7889c769000071b000084400001b00007c0063d012b7284",
            "application_token": "831c76b092f9f135d9b6b36c3a720757"
        }
    }
    ```

- Group chat

    ```json
    {
        "event": "ONIMBOTMESSAGEUPDATE",
        "event_handler_id": "455",
        "data": {
            "BOT": {
                "571": {
                    "access_token": "2c1da06900071b00084400023bf0f107464a9d34dc68a6b185f",
                    "expires": "1772100908",
                    "expires_in": "3600",
                    "scope": "imbot",
                    "domain": "some-domain.bitrix24.com",
                    "server_endpoint": "https://oauth.bitrix.info/rest/",
                    "status": "F",
                    "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                    "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
                    "refresh_token": "1c9cc76900071b00084400023bf0f107bbc698c8eca4b3e89099f4",
                    "user_id": "571",
                    "client_id": "a7eff906dd1d950269258a599214f69e",
                    "application_token": "831c76b092f9f135d9b6b36c3a720757",
                    "AUTH": {
                        "access_token": "2c1da06900071b00084400023bf0f107464a9d34dc68a6b185f",
                        "expires": "1772100908",
                        "expires_in": "3600",
                        "scope": "imbot",
                        "domain": "some-domain.bitrix24.com",
                        "server_endpoint": "https://oauth.bitrix.info/rest/",
                        "status": "F",
                        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                        "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
                        "refresh_token": "1c9cc76900071b00084400023bf0f107bbc698c8eca4b3e89099f4",
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
                "MESSAGE": "create a task list",
                "MESSAGE_TYPE": "C",
                "CHAT_AUTHOR_ID": "27",
                "CHAT_ENTITY_TYPE": "THREAD",
                "CHAT_ENTITY_ID": "",
                "CHAT_ENTITY_DATA_1": "",
                "CHAT_ENTITY_DATA_2": "",
                "CHAT_ENTITY_DATA_3": "",
                "CHAT_USER_COUNT": "2",
                "MENTIONED_LIST": {
                    "571": "571"
                },
                "PLATFORM_CONTEXT": "web",
                "MESSAGE_ORIGINAL": "[USER=571]NewBot[/USER], create a task list",
                "TO_USER_ID": "571",
                "DIALOG_ID": "chat1157",
                "MESSAGE_ID": "84537",
                "CHAT_TYPE": "C",
                "LANGUAGE": "en"
            }
        },
        "ts": "1772097308",
        "auth": {
            "access_token": "2c1da06900071b00084400001b00077e03d155e8",
            "expires": "1772100908",
            "expires_in": "3600",
            "scope": "imbot",
            "domain": "some-domain.bitrix24.com",
            "server_endpoint": "https://oauth.bitrix.info/rest/",
            "status": "F",
            "client_endpoint": "https://some-domain.bitrix24.com/rest/",
            "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
            "user_id": "27",
            "refresh_token": "1c9cc76900071b00084400001b00007b2dbee904c1",
            "application_token": "831c76b092f9f135d9b6b36c3a720757"
        }
    }
    ```

{% endlist %}

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../../data-types.md) | Event character code.

In this case — `ONIMBOTMESSAGEUPDATE` ||
|| **event_handler_id**
[`integer`](../../../../data-types.md) | Identifier of the event handler. ||
|| **data**
[`object`](../../../../data-types.md) | Object containing event data.

The structure is described [below](#data) ||
|| **ts**
[`timestamp`](../../../../data-types.md) | Date and time the event was sent from the [event queue](../../../../events/index.md) ||
|| **auth**
[`object`](../../../../data-types.md) | Object with authorization parameters of the user on behalf of whom the event was triggered.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

#|
|| **Parameter**
`type` | **Description** ||
|| **BOT**
[`object`](../../../../data-types.md) | Set of authorization parameters for the bots to which the event is addressed. The object key is the bot identifier `BOT_ID`.

The structure is described [below](#bot) ||
|| **PARAMS**
[`object`](../../../../data-types.md) | Parameters of the updated message.

The structure is described [below](#params) ||
|| **USER**
[`object`](../../../../data-types.md) | Data of the message author. May be absent or empty.

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
[`timestamp`](../../../../data-types.md) | Moment when the token expires ||
|| **expires_in**
[`integer`](../../../../data-types.md) | Token lifetime in seconds ||
|| **scope**
[`string`](../../../../data-types.md) | Scope within which the event occurred ||
|| **domain**
[`string`](../../../../data-types.md) | Bitrix24 address where the event occurred ||
|| **server_endpoint**
[`string`](../../../../data-types.md) | Address of the OAuth server for REST requests ||
|| **status**
[`string`](../../../../data-types.md) | Application status indicator on the account ||
|| **client_endpoint**
[`string`](../../../../data-types.md) | General path for REST API method calls for the account where the event occurred ||
|| **member_id**
[`string`](../../../../data-types.md) | Unique identifier of Bitrix24 ||
|| **refresh_token**
[`string`](../../../../data-types.md) | OAuth token for extending bot authorization ||
|| **user_id**
[`integer`](../../../../data-types.md) | Identifier of the bot user ||
|| **client_id**
[`string`](../../../../data-types.md) | Application identifier issued upon registration ||
|| **application_token**
[`string`](../../../../data-types.md) | Application token ||
|| **AUTH**
[`object`](../../../../data-types.md) | Bot authorization parameters in `auth` format.

The structure is described [below](#auth) ||
|| **BOT_ID**
[`integer`](../../../../data-types.md) | Identifier of the bot ||
|| **BOT_CODE**
[`string`](../../../../data-types.md) | Character code of the bot ||
|#

### Parameter PARAMS {#params}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`](../../../../data-types.md) | Identifier of the message ||
|| **CHAT_ID**
[`integer`](../../../../data-types.md) | Identifier of the chat ||
|| **AUTHOR_ID**
[`integer`](../../../../data-types.md) | Identifier of the message author ||
|| **MESSAGE**
[`string`](../../../../data-types.md) | Text of the message after the update.

In a group chat, the text is transmitted without BB-code mentions of the bot ||
|| **MESSAGE_TYPE**
[`string`](../../../../data-types.md) | Type of message

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
|| **FROM_USER_ID**
[`integer`](../../../../data-types.md) | Identifier of the message sender. This field is filled for personal chats ||
|| **TO_USER_ID**
[`integer`](../../../../data-types.md) | Identifier of the recipient.

In a personal chat, this is the identifier of the bot; in a group chat, it is the identifier of the user or bot to whom the message is addressed.

The value `0` means all participants in the chat ||
|| **CHAT_USER_COUNT**
[`integer`](../../../../data-types.md) | Number of participants in the chat ||
|| **MENTIONED_LIST**
[`object`](../../../../data-types.md) | Mentions of users in the message. This parameter depends on the chat type and is absent in personal chats.

The structure is described [below](#mentioned-list) ||
|| **PLATFORM_CONTEXT**
[`string`](../../../../data-types.md) | Platform from which the message was sent ||
|| **MESSAGE_ORIGINAL**
[`string`](../../../../data-types.md) | Original text of the message with BB-code mentions. This parameter depends on the chat type and is absent in personal chats ||
|| **DIALOG_ID**
[`string`](../../../../data-types.md) | Identifier of the dialog ||
|| **MESSAGE_ID**
[`integer`](../../../../data-types.md) | Identifier of the message. Duplicates the `ID` field ||
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

#### Parameter MENTIONED_LIST {#mentioned-list}

#|
|| **Parameter**
`type` | **Description** ||
|| **{USER_ID}**
[`integer`](../../../../data-types.md) | Identifier of the user or bot mentioned in the message ||
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
[`string`](../../../../data-types.md) | Position of the user ||
|| **GENDER**
[`string`](../../../../data-types.md) | Gender of the user: `M` or `F` ||
|| **IS_BOT**
[`string`](../../../../data-types.md) | Indicator of a bot user: `Y` or `N` ||
|| **IS_CONNECTOR**
[`string`](../../../../data-types.md) | Indicator of a connector user: `Y` or `N` ||
|| **IS_NETWORK**
[`string`](../../../../data-types.md) | Indicator of an external network user: `Y` or `N` ||
|| **IS_EXTRANET**
[`string`](../../../../data-types.md) | Indicator of an extranet user: `Y` or `N` ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [Event on Message Sent ONIMBOTMESSAGEADD](./on-imbot-message-add.md)
- [Event on Message Deletion ONIMBOTMESSAGEDELETE](./on-imbot-message-delete.md)
- [Send Message imbot.message.add](../imbot-message-add.md)
- [Update Sent Message imbot.message.update](../imbot-message-update.md)
- [Delete Message imbot.message.delete](../imbot-message-delete.md)
- [Create a Chat-Bot imbot.register](../../bots/imbot-register.md)
