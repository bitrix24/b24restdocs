# Event When Adding a Bot to a Chat ONIMBOTJOINCHAT

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imbot`](../../../../scopes/permissions.md)
>
> Who can subscribe: user of the application that registered the chat bot

{% note warning "DEPRECATED" %}

Development of this event has been halted. Please use [ONIMBOTV2JOINCHAT](../../../chat-bots-v2/imbot.v2/events/events.md#onimbotv2joinchat).

{% endnote %}

The `ONIMBOTJOINCHAT` event is triggered when a bot is added to a chat.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

{% list tabs %}

- Personal chat with the bot

    ```json
    {
        "event": "ONIMBOTJOINCHAT",
        "event_handler_id": "459",
        "data": {
            "BOT": {
                "571": {
                    "access_token": "e703a069000071b00084400023bf0f10751a702af1e",
                    "expires": "1772094439",
                    "expires_in": "3600",
                    "scope": "imbot",
                    "domain": "some-domain.bitrix24.com",
                    "server_endpoint": "https://oauth.bitrix.info/rest/",
                    "status": "F",
                    "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                    "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
                    "refresh_token": "d782c76900071b00084400023bf0f1077047d2feeb6c5f3fb",
                    "user_id": "571",
                    "client_id": "a7eff906dd1d950269258a599214f69e",
                    "application_token": "831c76b092f9f135d9b6b36c3a720757",
                    "AUTH": {
                        "access_token": "e703a069000071b00084400023bf0f10751a702af1e",
                        "expires": "1772094439",
                        "expires_in": "3600",
                        "scope": "imbot",
                        "domain": "some-domain.bitrix24.com",
                        "server_endpoint": "https://oauth.bitrix.info/rest/",
                        "status": "F",
                        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                        "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
                        "refresh_token": "d782c76900071b00084400023bf0f1077047d2feeb6c5f3fb",
                        "user_id": "571",
                        "client_id": "a7eff906dd1d950269258a599214f69e",
                        "application_token": "831c76b092f9f135d9b6b36c3a720757"
                    },
                    "BOT_ID": "571",
                    "BOT_CODE": "BOT"
                }
            },
            "PARAMS": {
                "CHAT_TYPE": "P",
                "MESSAGE_TYPE": "P",
                "BOT_ID": "571",
                "USER_ID": "27",
                "TO_USER_ID": "27",
                "FROM_USER_ID": "571",
                "DIALOG_ID": "27",
                "LANGUAGE": "de"
            },
            "USER": {
                "ID": "27",
                "NAME": "Svetlana Ivanova",
                "FIRST_NAME": "Svetlana",
                "LAST_NAME": "Ivanova",
                "WORK_POSITION": "",
                "GENDER": "F"
            }
        },
        "ts": "1772090839",
        "auth": {
            "access_token": "e703a069000071b00084400001b00074523806a5537056abff",
            "expires": "1772094439",
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

- Group chat

    ```json
    {
        "event": "ONIMBOTJOINCHAT",
        "event_handler_id": "459",
        "data": {
            "BOT": {
                "571": {
                    "access_token": "4d12a06900071b00084400023bf0f1079c6f8b9190c698fd2",
                    "expires": "1772098125",
                    "expires_in": "3600",
                    "scope": "imbot",
                    "domain": "some-domain.bitrix24.com",
                    "server_endpoint": "https://oauth.bitrix.info/rest/",
                    "status": "F",
                    "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                    "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
                    "refresh_token": "3d91c7690000071b00084400023bf0f107580dad11018e",
                    "user_id": "571",
                    "client_id": "a7eff906dd1d950269258a599214f69e",
                    "application_token": "831c76b092f9f135d9b6b36c3a720757",
                    "AUTH": {
                        "access_token": "4d12a06900071b00084400023bf0f1079c6f8b9190c698fd2",
                        "expires": "1772098125",
                        "expires_in": "3600",
                        "scope": "imbot",
                        "domain": "some-domain.bitrix24.com",
                        "server_endpoint": "https://oauth.bitrix.info/rest/",
                        "status": "F",
                        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                        "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
                        "refresh_token": "3d91c7690000071b00084400023bf0f107580dad11018e",
                        "user_id": "571",
                        "client_id": "a7eff906dd1d950269258a599214f69e",
                        "application_token": "831c76b092f9f135d9b6b36c3a720757"
                    },
                    "BOT_ID": "571",
                    "BOT_CODE": "BOT"
                }
            },
            "PARAMS": {
                "CHAT_TYPE": "C",
                "MESSAGE_TYPE": "C",
                "BOT_ID": "571",
                "USER_ID": "27",
                "CHAT_ID": "1157",
                "CHAT_AUTHOR_ID": "27",
                "CHAT_ENTITY_TYPE": "THREAD",
                "CHAT_ENTITY_ID": "",
                "ACCESS_HISTORY": "1",
                "DIALOG_ID": "chat1157",
                "LANGUAGE": "de"
            },
            "USER": {
                "ID": "27",
                "NAME": "Svetlana Ivanova",
                "FIRST_NAME": "Svetlana",
                "LAST_NAME": "Ivanova",
                "WORK_POSITION": "",
                "GENDER": "F"
            }
        },
        "ts": "1772094525",
        "auth": {
            "access_token": "4e12a06900071b00084400001b000070de69612254f5f11a912b908",
            "expires": "1772098126",
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

{% endlist %}

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../../data-types.md) | Symbolic code of the event.

In this case — `ONIMBOTJOINCHAT` ||
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
[`object`](../../../../data-types.md) | Set of authorization parameters for the bots to which the event is directed. The key of the object is the bot identifier `BOT_ID`.

The structure is described [below](#bot) ||
|| **PARAMS**
[`object`](../../../../data-types.md) | Event parameters.

The structure is described [below](#params) ||
|| **USER**
[`object`](../../../../data-types.md) | Data of the user who added the bot to the chat. It may be an empty object if `ID = 0`.

The structure is described [below](#user) ||
|#

### Parameter BOT {#bot}

#|
|| **Parameter**
`type` | **Description** ||
|| **/{BOT_ID/}**
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
[`integer`](../../../../data-types.md) | Lifetime of the token in seconds ||
|| **scope**
[`string`](../../../../data-types.md) | Scope within which the event occurred ||
|| **domain**
[`string`](../../../../data-types.md) | Address of Bitrix24 where the event occurred ||
|| **server_endpoint**
[`string`](../../../../data-types.md) | Address of the OAuth server for REST requests ||
|| **status**
[`string`](../../../../data-types.md) | Indicator of the application's state on the account ||
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
|| **CHAT_TYPE**
[`string`](../../../../data-types.md) | Type of chat to which the bot was added.

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
- `X` — external chat  ||
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
- `X` — external chat  ||
|| **BOT_ID**
[`integer`](../../../../data-types.md) | Identifier of the bot ||
|| **USER_ID**
[`integer`](../../../../data-types.md) | Identifier of the user who added the bot ||
|| **TO_USER_ID**
[`integer`](../../../../data-types.md) | Identifier of the user with whom the personal dialogue was created. This parameter is only for personal chat ||
|| **FROM_USER_ID**
[`integer`](../../../../data-types.md) | Identifier of the bot in the personal dialogue. This parameter is only for personal chat ||
|| **CHAT_ID**
[`integer`](../../../../data-types.md) | Identifier of the group chat. This parameter is only for group chat ||
|| **CHAT_AUTHOR_ID**
[`integer`](../../../../data-types.md) | Identifier of the owner of the group chat. This parameter is only for group chat ||
|| **CHAT_ENTITY_TYPE**
[`string`](../../../../data-types.md) | Type of the object to which the group chat is linked. This parameter is only for group chat ||
|| **CHAT_ENTITY_ID**
[`string`](../../../../data-types.md) | Identifier of the object to which the group chat is linked. This parameter is only for group chat ||
|| **ACCESS_HISTORY**
[`integer`](../../../../data-types.md) | Indicator of the bot's access to history: `1` — access granted, `0` — no access. This parameter is only for group chat ||
|| **SILENT_JOIN**
[`string`](../../../../data-types.md) | Indicator of adding the bot without a system message: `Y` or `N`. This parameter is only for group chat Copilot ||
|| **DIALOG_ID**
[`string`](../../../../data-types.md) | Identifier of the dialogue ||
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
[`string`](../../../../data-types.md) | Position of the user ||
|| **GENDER**
[`string`](../../../../data-types.md) | Gender of the user: `M` or `F` ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../../../_includes/auth-params-in-events.md) %}

## Continue Exploring

- [Event on Message Sent ONIMBOTMESSAGEADD](../../messages/events/on-imbot-message-add.md)
- [Event on Message Update ONIMBOTMESSAGEUPDATE](../../messages/events/on-imbot-message-update.md)
- [Event on Message Deletion ONIMBOTMESSAGEDELETE](../../messages/events/on-imbot-message-delete.md)
- [Event for Deleting Chat-Bot ONIMBOTDELETE](../../events/on-imbot-delete.md)
