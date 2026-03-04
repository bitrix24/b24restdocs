# Event Triggered by the Chat Bot Command ONIMCOMMANDADD

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can subscribe: user of the application that registered the chat bot

The `ONIMCOMMANDADD` event is triggered when a user invokes a bot command in a chat.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

{% list tabs %}

- Personal chat with the bot

    ```json
    {
        "event": "ONIMCOMMANDADD",
        "event_handler_id": "1045",
        "data": {
            "COMMAND": {
                "103": {
                    "access_token": "be4ca0690000071b006e2cf200050b0077317605eb83b25d2bd6",
                    "expires": "1772113086",
                    "expires_in": "3600",
                    "scope": "imbot",
                    "domain": "some-domain.bitrix24.com",
                    "server_endpoint": "https://oauth.bitrix.info/rest/",
                    "status": "F",
                    "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                    "member_id": "d897063e1ce7c5eb9f04b9751eef5915",
                    "refresh_token": "aecbc7690000071b006e2cf200050b007f827f47e120e49da",
                    "user_id": "1291",
                    "client_id": "a7eff906dd1d950269258a599214f69e",
                    "application_token": "8e3ab48a764a9b02700e759d59e91125",
                    "AUTH": {
                        "access_token": "be4ca0690000071b006e2cf200050b0077317605eb83b25d2bd6",
                        "expires": "1772113086",
                        "expires_in": "3600",
                        "scope": "imbot",
                        "domain": "some-domain.bitrix24.com",
                        "server_endpoint": "https://oauth.bitrix.info/rest/",
                        "status": "F",
                        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                        "member_id": "d897063e1ce7c5eb9f04b9751eef5915",
                        "refresh_token": "aecbc7690000071b006e2cf200050b007f827f47e120e49da",
                        "user_id": "1291",
                        "client_id": "a7eff906dd1d950269258a599214f69e",
                        "application_token": "8e3ab48a764a9b02700e759d59e91125"
                    },
                    "BOT_ID": "1291",
                    "BOT_CODE": "BOT_GM",
                    "COMMAND": "echo",
                    "COMMAND_ID": "103",
                    "COMMAND_PARAMS": "test",
                    "COMMAND_CONTEXT": "TEXTAREA",
                    "MESSAGE_ID": "33899"
                }
            },
            "PARAMS": {
                "MESSAGE": "/echo test",
                "TEMPLATE_ID": "acae8d22-4536-4f47-a276-2e3bdf866475",
                "MESSAGE_TYPE": "P",
                "FROM_USER_ID": "1269",
                "DIALOG_ID": "1269",
                "TO_CHAT_ID": "2737",
                "AUTHOR_ID": "1269",
                "SYSTEM": "N",
                "TO_USER_ID": "1291",
                "SKIP_USER_CHECK": "Y",
                "PUSH": "Y",
                "PUSH_IMPORTANT": "N",
                "RECENT_ADD": "Y",
                "RECENT_SKIP_AUTHOR": "N",
                "CONVERT": "N",
                "SKIP_COMMAND": "N",
                "SKIP_COUNTER_INCREMENTS": "N",
                "SILENT_CONNECTOR": "N",
                "SKIP_CONNECTOR": "N",
                "IMPORTANT_CONNECTOR": "N",
                "NO_SESSION_OL": "N",
                "FAKE_RELATION": "0",
                "SKIP_URL_INDEX": "N",
                "MESSAGE_ID": "33899",
                "CHAT_TYPE": "P",
                "LANGUAGE": "en"
            },
            "USER": {
                "ID": "1269",
                "NAME": "John Smith",
                "FIRST_NAME": "John",
                "LAST_NAME": "Smith",
                "GENDER": "M"
            }
        },
        "ts": "1772109486",
        "auth": {
            "access_token": "be4ca06900071b006e2cf200004f50005243fd374178cf59f7831",
            "expires": "1772113086",
            "expires_in": "3600",
            "scope": "imbot",
            "domain": "some-domain.bitrix24.com",
            "server_endpoint": "https://oauth.bitrix.info/rest/",
            "status": "F",
            "client_endpoint": "https://some-domain.bitrix24.com/rest/",
            "member_id": "d897063e1ce7c5eb9f04b9751eef5915",
            "user_id": "1269",
            "refresh_token": "aecbc76900071b006e2cf200004f50000f2e62ee8e8baa598968",
            "application_token": "8e3ab48a764a9b02700e759d59e91125"
        }
    }
    ```

- Group chat

    ```json
    {
        "event": "ONIMCOMMANDADD",
        "event_handler_id": "1035",
        "data": {
            "COMMAND": {
                "99": {
                    "access_token": "f1ca0690000071b006e2cf20050b00074d9ed82e255b",
                    "expires": "1772100799",
                    "expires_in": "3600",
                    "scope": "imbot",
                    "domain": "some-domain.bitrix24.com",
                    "server_endpoint": "https://oauth.bitrix.info/rest/",
                    "status": "F",
                    "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                    "member_id": "d897063e1ce7c5eb9f04b9751eef5915",
                    "refresh_token": "af9bc7690000071b006e2cf2050b00074d5d4c29337afbc6bb",
                    "user_id": "1291",
                    "client_id": "a7eff906dd1d950269258a599214f69e",
                    "application_token": "8e3ab48a764a9b02700e759d59e91125",
                    "AUTH": {
                        "access_token": "f1ca0690000071b006e2cf20050b00074d9ed82e255b",
                        "expires": "1772100799",
                        "expires_in": "3600",
                        "scope": "imbot",
                        "domain": "some-domain.bitrix24.com",
                        "server_endpoint": "https://oauth.bitrix.info/rest/",
                        "status": "F",
                        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                        "member_id": "d897063e1ce7c5eb9f04b9751eef5915",
                        "refresh_token": "af9bc7690000071b006e2cf2050b00074d5d4c29337afbc6bb",
                        "user_id": "1291",
                        "client_id": "a7eff906dd1d950269258a599214f69e",
                        "application_token": "8e3ab48a764a9b02700e759d59e91125"
                    },
                    "BOT_ID": "1291",
                    "BOT_CODE": "BOT_GM",
                    "COMMAND": "echo",
                    "COMMAND_ID": "99",
                    "COMMAND_PARAMS": "test",
                    "COMMAND_CONTEXT": "TEXTAREA",
                    "MESSAGE_ID": "33871"
                }
            },
            "PARAMS": {
                "MESSAGE": "/echo test",
                "TEMPLATE_ID": "eb3d5b1c-0830-4728-a7a3-73e9745a90a1",
                "MESSAGE_TYPE": "C",
                "FROM_USER_ID": "1269",
                "DIALOG_ID": "chat2725",
                "TO_CHAT_ID": "2725",
                "AUTHOR_ID": "1269",
                "SYSTEM": "N",
                "CHAT_ID": "2725",
                "CHAT_PARENT_ID": "0",
                "CHAT_PARENT_MID": "0",
                "CHAT_TITLE": "New chat name",
                "CHAT_AUTHOR_ID": "1291",
                "CHAT_TYPE": "C",
                "CHAT_AVATAR": "33079",
                "CHAT_COLOR": "GREEN",
                "CHAT_ENTITY_TYPE": "CHAT",
                "CHAT_ENTITY_ID": "13",
                "CHAT_ENTITY_DATA_1": "",
                "CHAT_ENTITY_DATA_2": "",
                "CHAT_ENTITY_DATA_3": "",
                "CHAT_EXTRANET": "N",
                "CHAT_PREV_MESSAGE_ID": "0",
                "CHAT_CAN_POST": "MEMBER",
                "RID": "1269",
                "IS_MANAGER": "N",
                "BOT_IN_CHAT": {
                    "1291": "1291"
                },
                "SKIP_USER_CHECK": "Y",
                "PUSH": "Y",
                "PUSH_IMPORTANT": "N",
                "RECENT_ADD": "Y",
                "RECENT_SKIP_AUTHOR": "N",
                "CONVERT": "N",
                "SKIP_COMMAND": "N",
                "SKIP_COUNTER_INCREMENTS": "N",
                "SILENT_CONNECTOR": "N",
                "SKIP_CONNECTOR": "N",
                "IMPORTANT_CONNECTOR": "N",
                "NO_SESSION_OL": "N",
                "FAKE_RELATION": "0",
                "SKIP_URL_INDEX": "N",
                "MESSAGE_ID": "33871",
                "LANGUAGE": "en"
            },
            "USER": {
                "ID": "1269",
                "NAME": "John Smith",
                "FIRST_NAME": "John",
                "LAST_NAME": "Smith",
                "GENDER": "M"
            }
        },
        "ts": "1772097199",
        "auth": {
            "access_token": "bf1ca06900071b006e2cf200004f500031d5d527f58a97b0768",
            "expires": "1772100799",
            "expires_in": "3600",
            "scope": "imbot",
            "domain": "some-domain.bitrix24.com",
            "server_endpoint": "https://oauth.bitrix.info/rest/",
            "status": "F",
            "client_endpoint": "https://some-domain.bitrix24.com/rest/",
            "member_id": "d897063e1ce7c5eb9f04b9751eef5915",
            "user_id": "1269",
            "refresh_token": "af9bc76900071b006e2cf200004f50080d2934573350daabf8",
            "application_token": "8e3ab48a764a9b02700e759d59e91125"
        }
    }
    ```

{% endlist %}

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `ONIMCOMMANDADD` ||
|| **event_handler_id**
[`integer`](../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../data-types.md) | Object containing event data.

The structure is described [below](#data) ||
|| **ts**
[`timestamp`](../../../data-types.md) | Date and time the event was sent from the [event queue](../../../events/index.md) ||
|| **auth**
[`object`](../../../data-types.md) | Object containing the authorization parameters of the user on behalf of whom the event was triggered.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

#|
|| **Parameter**
`type` | **Description** ||
|| **COMMAND**
[`object`](../../../data-types.md) | Set of invoked commands of the chat bot.

The structure is described [below](#command) ||
|| **PARAMS**
[`object`](../../../data-types.md) | Parameters of the message in which the command was invoked.

The structure is described [below](#params) ||
|| **USER**
[`object`](../../../data-types.md) | Data of the message author. It may be an empty object if `FROM_USER_ID = 0`.

The structure is described [below](#user) ||
|#

### Parameter COMMAND {#command}

#|
|| **Parameter**
`type` | **Description** ||
|| **{COMMAND_ID}**
[`object`](../../../data-types.md) | Data object of a specific invoked command. The key corresponds to the command identifier, for example `103`.

The structure is described [below](#command-item) ||
|#

#### Element \{COMMAND_ID\} {#command-item}

#|
|| **Parameter**
`type` | **Description** ||
|| **access_token**
[`string`](../../../data-types.md) | OAuth authorization token of the bot ||
|| **expires**
[`timestamp`](../../../data-types.md) | Moment of token expiration ||
|| **expires_in**
[`integer`](../../../data-types.md) | Token lifetime in seconds ||
|| **scope**
[`string`](../../../data-types.md) | Scope within which the event occurred ||
|| **domain**
[`string`](../../../data-types.md) | Address of Bitrix24 where the event occurred ||
|| **server_endpoint**
[`string`](../../../data-types.md) | Address of the OAuth server for REST requests ||
|| **status**
[`string`](../../../data-types.md) | Application status indicator on the account ||
|| **client_endpoint**
[`string`](../../../data-types.md) | Base path for calling the REST API on the current account ||
|| **member_id**
[`string`](../../../data-types.md) | Unique identifier of Bitrix24 ||
|| **refresh_token**
[`string`](../../../data-types.md) | OAuth token for renewing the bot's authorization ||
|| **user_id**
[`integer`](../../../data-types.md) | Identifier of the bot user ||
|| **client_id**
[`string`](../../../data-types.md) | Identifier of the application issued upon registration ||
|| **application_token**
[`string`](../../../data-types.md) | Application token ||
|| **AUTH**
[`object`](../../../data-types.md) | Authorization parameters of the bot in `auth` format.

The structure is described [below](#auth) ||
|| **BOT_ID**
[`integer`](../../../data-types.md) | Identifier of the bot ||
|| **BOT_CODE**
[`string`](../../../data-types.md) | Symbolic code of the bot ||
|| **COMMAND**
[`string`](../../../data-types.md) | Symbolic code of the invoked command ||
|| **COMMAND_ID**
[`integer`](../../../data-types.md) | Identifier of the invoked command ||
|| **COMMAND_PARAMS**
[`string`](../../../data-types.md) | Parameters with which the command was invoked ||
|| **COMMAND_CONTEXT**
[`string`](../../../data-types.md) | Context of the command invocation.

Possible values:
- `TEXTAREA` — command entered manually
- `KEYBOARD` — command invoked by keyboard button ||
|| **MESSAGE_ID**
[`integer`](../../../data-types.md) | Identifier of the message to which a response is required ||
|#

### Parameter PARAMS {#params}

#|
|| **Parameter**
`type` | **Description** ||
|| **MESSAGE**
[`string`](../../../data-types.md) | Text of the message with the command ||
|| **TEMPLATE_ID**
[`string`](../../../data-types.md) | Identifier of the message template ||
|| **MESSAGE_TYPE**
[`string`](../../../data-types.md) | Type of the message.

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
|| **FROM_USER_ID**
[`integer`](../../../data-types.md) | Identifier of the message sender ||
|| **DIALOG_ID**
[`string`](../../../data-types.md) | Identifier of the dialog ||
|| **TO_CHAT_ID**
[`integer`](../../../data-types.md) | Identifier of the chat ||
|| **AUTHOR_ID**
[`integer`](../../../data-types.md) | Identifier of the message author ||
|| **SYSTEM**
[`string`](../../../data-types.md) | Indicator of a system message: `Y` or `N` ||
|| **TO_USER_ID**
[`integer`](../../../data-types.md) | Identifier of the interlocutor in a personal chat. This parameter is available only for personal chats ||
|| **SKIP_USER_CHECK**
[`string`](../../../data-types.md) | Skip user check: `Y` or `N` ||
|| **PUSH**
[`string`](../../../data-types.md) | Indicator of sending a push notification: `Y` or `N` ||
|| **PUSH_IMPORTANT**
[`string`](../../../data-types.md) | Indicator of an important push notification: `Y` or `N` ||
|| **RECENT_ADD**
[`string`](../../../data-types.md) | Add message to the Recent chats list: `Y` or `N` ||
|| **RECENT_SKIP_AUTHOR**
[`string`](../../../data-types.md) | Exclude author from updating the Recent chats list: `Y` or `N` ||
|| **CONVERT**
[`string`](../../../data-types.md) | Indicator of message formatting conversion: `Y` or `N` ||
|| **SKIP_COMMAND**
[`string`](../../../data-types.md) | Skip command processing in the message: `Y` or `N` ||
|| **SKIP_COUNTER_INCREMENTS**
[`string`](../../../data-types.md) | Do not increment unread message counters: `Y` or `N` ||
|| **SILENT_CONNECTOR**
[`string`](../../../data-types.md) | Send message to external connector without notification: `Y` or `N` ||
|| **SKIP_CONNECTOR**
[`string`](../../../data-types.md) | Do not send message to external connectors: `Y` or `N` ||
|| **IMPORTANT_CONNECTOR**
[`string`](../../../data-types.md) | Indicator of an important message for the connector: `Y` or `N` ||
|| **NO_SESSION_OL**
[`string`](../../../data-types.md) | Do not create an Open Line session: `Y` or `N` ||
|| **FAKE_RELATION**
[`integer`](../../../data-types.md) | System value of the user's relation to the chat ||
|| **SKIP_URL_INDEX**
[`string`](../../../data-types.md) | Do not index links from the message: `Y` or `N` ||
|| **MESSAGE_ID**
[`integer`](../../../data-types.md) | Identifier of the message ||
|| **CHAT_TYPE**
[`string`](../../../data-types.md) | Type of the chat.

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
[`string`](../../../data-types.md) | Default language of Bitrix24 ||
|| **CHAT_ID**
[`integer`](../../../data-types.md) | Identifier of the chat. This parameter is available only for group chats ||
|| **CHAT_PARENT_ID**
[`integer`](../../../data-types.md) | Identifier of the parent chat. This parameter is available only for group chats ||
|| **CHAT_PARENT_MID**
[`integer`](../../../data-types.md) | Identifier of the parent message. This parameter is available only for group chats ||
|| **CHAT_TITLE**
[`string`](../../../data-types.md) | Title of the chat. This parameter is available only for group chats ||
|| **CHAT_AUTHOR_ID**
[`integer`](../../../data-types.md) | Identifier of the chat owner. This parameter is available only for group chats ||
|| **CHAT_AVATAR**
[`string`](../../../data-types.md) | Identifier of the chat avatar. `0` — avatar not set. This parameter depends on the chat type, it is absent in personal dialogs ||
|| **CHAT_COLOR**
[`string`](../../../data-types.md) | Color scheme of the chat. This parameter depends on the chat type, it is absent in personal dialogs ||
|| **CHAT_ENTITY_TYPE**
[`string`](../../../data-types.md) | Type of the object to which the chat is linked. This parameter depends on the chat type, it is absent in personal dialogs ||
|| **CHAT_ENTITY_ID**
[`string`](../../../data-types.md) | Identifier of the object to which the chat is linked. This parameter depends on the chat type, it is absent in personal dialogs ||
|| **CHAT_ENTITY_DATA_1**
[`string`](../../../data-types.md) | Additional data of the chat object — field 1. This parameter depends on the chat type, it is absent in personal dialogs ||
|| **CHAT_ENTITY_DATA_2**
[`string`](../../../data-types.md) | Additional data of the chat object — field 2. This parameter depends on the chat type, it is absent in personal dialogs ||
|| **CHAT_ENTITY_DATA_3**
[`string`](../../../data-types.md) | Additional data of the chat object — field 3. This parameter depends on the chat type, it is absent in personal dialogs ||
|| **CHAT_EXTRANET**
[`string`](../../../data-types.md) | Indicator of an extranet chat: `Y` or `N`. This parameter depends on the chat type, it is absent in personal dialogs ||
|| **CHAT_PREV_MESSAGE_ID**
[`integer`](../../../data-types.md) | Identifier of the previous message in the chat. This parameter depends on the chat type, it is absent in personal dialogs ||
|| **CHAT_CAN_POST**
[`string`](../../../data-types.md) | Permission of the current user to send messages in the chat. This parameter depends on the chat type, it is absent in personal dialogs ||
|| **RID**
[`integer`](../../../data-types.md) | Identifier of the record of the user's relation to the chat. This parameter depends on the chat type, it is absent in personal dialogs ||
|| **IS_MANAGER**
[`string`](../../../data-types.md) | Indicator of the manager role: `Y` or `N`. This parameter is available only for group chats ||
|| **BOT_IN_CHAT**
[`object`](../../../data-types.md) | List of bots present in the chat. This parameter is available only for group chats.

The structure is described [below](#bot-in-chat) ||
|#

#### Parameter BOT_IN_CHAT {#bot-in-chat}

#|
|| **Parameter**
`type` | **Description** ||
|| **{BOT_ID}**
[`integer`](../../../data-types.md) | Identifier of the bot present in the chat ||
|#

### Parameter USER {#user}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the user ||
|| **NAME**
[`string`](../../../data-types.md) | User's full name ||
|| **FIRST_NAME**
[`string`](../../../data-types.md) | User's first name ||
|| **LAST_NAME**
[`string`](../../../data-types.md) | User's last name ||
|| **GENDER**
[`string`](../../../data-types.md) | User's gender: `M` or `F` ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../chats/events/on-imbot-join-chat.md)
- [{#T}](../../messages/events/on-imbot-message-add.md)
- [{#T}](../../messages/events/on-imbot-message-update.md)
- [{#T}](../../messages/events/on-imbot-message-delete.md)