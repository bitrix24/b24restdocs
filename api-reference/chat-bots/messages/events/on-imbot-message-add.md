# Event on Sending Message ONIMBOTMESSAGEADD

> Scope: [`imbot`](../../../scopes/permissions.md)
>
> Who can subscribe: user of the application that registered the chat bot

The `ONIMBOTMESSAGEADD` event is triggered when a message is sent in a dialogue with the chat bot. In a group chat, the event is triggered if the chat bot is mentioned.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

{% list tabs %}

- Personal chat with the bot

    ```json
    {
        "event": "ONIMBOTMESSAGEADD",
        "event_handler_id": "403",
        "data": {
            "BOT": {
                "567": {
                    "access_token": "a98b9d690000071b0000084400000237f0f107589d1e",
                    "expires": "1771932585",
                    "expires_in": "3600",
                    "scope": "imbot",
                    "domain": "some-domain.bitrix24.com",
                    "server_endpoint": "https://oauth.bitrix.info/rest/",
                    "status": "F",
                    "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                    "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
                    "refresh_token": "990ac5690000071b0000084400000237",
                    "user_id": "567",
                    "client_id": "a7eff906dd1d950269258a599214f69e",
                    "application_token": "831c76b092f9f135d9b6b36c3a720757",
                    "AUTH": {
                        "access_token": "a98b9d690000071b0000084400000237f0f107589d1e",
                        "expires": "1771932585",
                        "expires_in": "3600",
                        "scope": "imbot",
                        "domain": "some-domain.bitrix24.com",
                        "server_endpoint": "https://oauth.bitrix.info/rest/",
                        "status": "F",
                        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                        "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
                        "refresh_token": "990ac5690000071b0000084400000237",
                        "user_id": "567",
                        "client_id": "a7eff906dd1d950269258a599214f69e",
                        "application_token": "831c76b092f9f135d9b6b36c3a720757"
                    },
                    "BOT_ID": "567",
                    "BOT_CODE": "BOT1"
                }
            },
            "PARAMS": {
                "MESSAGE": "Hello",
                "TEMPLATE_ID": "796c45aa-70e5-45aa-930c-c7fb32710c62",
                "MESSAGE_TYPE": "P",
                "FROM_USER_ID": "27",
                "DIALOG_ID": "27",
                "TO_CHAT_ID": "1407",
                "AUTHOR_ID": "27",
                "SYSTEM": "N",
                "TO_USER_ID": "567",
                "PUSH": "Y",
                "PUSH_IMPORTANT": "N",
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
                "COMMAND_CONTEXT": "TEXTAREA",
                "CHAT_USER_COUNT": "2",
                "PLATFORM_CONTEXT": "web",
                "MESSAGE_ID": "84331",
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
        "ts": "1771928985",
        "auth": {
            "access_token": "a98b9d690000071b000008440000001b0000071d8a2",
            "expires": "1771932585",
            "expires_in": "3600",
            "scope": "imbot",
            "domain": "some-domain.bitrix24.com",
            "server_endpoint": "https://oauth.bitrix.info/rest/",
            "status": "F",
            "client_endpoint": "https://some-domain.bitrix24.com/rest/",
            "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
            "user_id": "27",
            "refresh_token": "990ac569071b000008440000001b000007f80ccc35b5e3",
            "application_token": "831c76b092f9f135d9b66c3a720757"
        }
    }
    ```

- Group chat

    ```json
    {
        "event": "ONIMBOTMESSAGEADD",
        "event_handler_id": "403",
        "data": {
            "BOT": {
                "567": {
                    "access_token": "ccb69d690000071b0000084400000237f0f10799e08fc9016818e8b51ecc9f7b5342a5",
                    "expires": "1771943628",
                    "expires_in": "3600",
                    "scope": "imbot",
                    "domain": "some-domain.bitrix24.com",
                    "server_endpoint": "https://oauth.bitrix.info/rest/",
                    "status": "F",
                    "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                    "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
                    "refresh_token": "bc35c5690000071b0000084400000237f0f107abd386665cf9cfc96eaf25e017730bc5",
                    "user_id": "567",
                    "client_id": "a7eff906dd1d950269258a599214f69e",
                    "application_token": "831c76b092f9f135d9b6b36c3a720757",
                    "AUTH": {
                        "access_token": "ccb69d690000071b0000084400000237f0f10799e08fc9016818e8b51ecc9f7b5342a5",
                        "expires": "1771943628",
                        "expires_in": "3600",
                        "scope": "imbot",
                        "domain": "some-domain.bitrix24.com",
                        "server_endpoint": "https://oauth.bitrix.info/rest/",
                        "status": "F",
                        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
                        "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
                        "refresh_token": "bc35c5690000071b0000084400000237f0f107abd386665cf9cfc96eaf25e017730bc5",
                        "user_id": "567",
                        "client_id": "a7eff906dd1d950269258a599214f69e",
                        "application_token": "831c76b092f9f135d9b6b36c3a720757"
                    },
                    "BOT_ID": "567",
                    "BOT_CODE": "BOT1"
                }
            },
            "PARAMS": {
                "MESSAGE": ", how to set up the left menu",
                "TEMPLATE_ID": "6d0eef7c-12b4-41e7-b709-887ba410ded3",
                "MESSAGE_TYPE": "C",
                "FROM_USER_ID": "27",
                "DIALOG_ID": "chat1157",
                "TO_CHAT_ID": "1157",
                "AUTHOR_ID": "27",
                "SYSTEM": "N",
                "CHAT_ID": "1157",
                "CHAT_TITLE": "Brown Chat #18",
                "CHAT_AUTHOR_ID": "27",
                "CHAT_TYPE": "C",
                "CHAT_AVATAR": "0",
                "CHAT_COLOR": "BROWN",
                "CHAT_ENTITY_TYPE": "THREAD",
                "CHAT_ENTITY_ID": "",
                "CHAT_ENTITY_DATA_1": "",
                "CHAT_ENTITY_DATA_2": "",
                "CHAT_ENTITY_DATA_3": "",
                "CHAT_EXTRANET": "N",
                "CHAT_PREV_MESSAGE_ID": "80961",
                "CHAT_CAN_POST": "MEMBER",
                "RID": "27",
                "IS_MANAGER": "N",
                "PUSH": "Y",
                "PUSH_IMPORTANT": "N",
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
                "COMMAND_CONTEXT": "TEXTAREA",
                "CHAT_USER_COUNT": "2",
                "MENTIONED_LIST": {
                    "567": "567"
                },
                "PLATFORM_CONTEXT": "web",
                "MESSAGE_ORIGINAL": "[USER=567]NewBot[/USER] , how to set up the left menu",
                "TO_USER_ID": "567",
                "MESSAGE_ID": "84351",
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
        "ts": "1771940028",
        "auth": {
            "access_token": "ccb69d690000071b000008440000001b0000078c69a6b996b744c26b73e80ddeda0052",
            "expires": "1771943628",
            "expires_in": "3600",
            "scope": "imbot",
            "domain": "some-domain.bitrix24.com",
            "server_endpoint": "https://oauth.bitrix.info/rest/",
            "status": "F",
            "client_endpoint": "https://some-domain.bitrix24.com/rest/",
            "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
            "user_id": "27",
            "refresh_token": "bc35c5690000071b000008440000001b000007b7a839f68bc2cdd19de2677410425077",
            "application_token": "831c76b092f9f135d9b6b36c3a720757"
        }
    }
    ```

{% endlist %}

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `ONIMBOTMESSAGEADD` ||
|| **event_handler_id**
[`integer`](../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../data-types.md) | Object with event data.

The structure is described [below](#data) ||
|| **ts**
[`timestamp`](../../../data-types.md) | Date and time of sending the event from the [event queue](../../../events/index.md) ||
|| **auth**
[`object`](../../../data-types.md) | Object with authorization parameters of the user on behalf of whom the event was triggered.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

#|
|| **Parameter**
`type` | **Description** ||
|| **BOT**
[`object`](../../../data-types.md) | Set of authorization parameters for the bots to which the message is intended. The key of the object is the bot identifier `BOT_ID`.

The structure is described [below](#bot) ||
|| **PARAMS**
[`object`](../../../data-types.md) | Message parameters.

The structure is described [below](#params) ||
|| **USER**
[`object`](../../../data-types.md) | Data of the message author. It may be an empty object if `ID = 0`.

The structure is described [below](#user) ||
|#

### Parameter BOT {#bot}

#|
|| **Parameter**
`type` | **Description** ||
|| **\{BOT_ID\}**
[`object`](../../../data-types.md) | Data object of a specific bot. The key corresponds to the bot identifier, for example `567`.

The structure is described [below](#bot-item) ||
|#

#### Element \{BOT_ID\} {#bot-item}

#|
|| **Parameter**
`type` | **Description** ||
|| **access_token**
[`string`](../../../data-types.md) | OAuth authorization token for the bot ||
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
[`string`](../../../data-types.md) | Common path for calling REST API methods for Bitrix24 where the event occurred ||
|| **member_id**
[`string`](../../../data-types.md) | Unique identifier of Bitrix24 ||
|| **refresh_token**
[`string`](../../../data-types.md) | OAuth token for renewing bot authorization ||
|| **user_id**
[`integer`](../../../data-types.md) | Identifier of the bot user ||
|| **client_id**
[`string`](../../../data-types.md) | Application identifier issued upon registration ||
|| **application_token**
[`string`](../../../data-types.md) | Application token ||
|| **AUTH**
[`object`](../../../data-types.md) | Authorization parameters of the bot in `auth` format.

The structure is described [below](#auth) ||
|| **BOT_ID**
[`integer`](../../../data-types.md) | Identifier of the bot ||
|| **BOT_CODE**
[`string`](../../../data-types.md) | Symbolic code of the bot ||
|#

### Parameter PARAMS {#params}

#|
|| **Parameter**
`type` | **Description** ||
|| **MESSAGE**
[`string`](../../../data-types.md) | Text of the message ||
|| **TEMPLATE_ID**
[`string`](../../../data-types.md) | Identifier of the message template ||
|| **MESSAGE_TYPE**
[`string`](../../../data-types.md) | Type of message.

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
[`string`](../../../data-types.md) | Identifier of the dialogue ||
|| **TO_CHAT_ID**
[`integer`](../../../data-types.md) | Identifier of the chat ||
|| **AUTHOR_ID**
[`integer`](../../../data-types.md) | Identifier of the message author ||
|| **SYSTEM**
[`string`](../../../data-types.md) | Indicator of a system message: `Y` or `N` ||
|| **TO_USER_ID**
[`integer`](../../../data-types.md) | Identifier of the message recipient ||
|| **CHAT_ID**
[`integer`](../../../data-types.md) | Identifier of the chat. This parameter depends on the chat type and is absent in personal dialogue ||
|| **CHAT_TITLE**
[`string`](../../../data-types.md) | Title of the chat. This parameter depends on the chat type and is absent in personal dialogue ||
|| **CHAT_AUTHOR_ID**
[`integer`](../../../data-types.md) | Identifier of the chat owner. This parameter depends on the chat type and is absent in personal dialogue ||
|| **CHAT_TYPE**
[`string`](../../../data-types.md) | Type of chat. 

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
|| **CHAT_AVATAR**
[`string`](../../../data-types.md) | Identifier of the chat avatar. `0` — avatar not set. This parameter depends on the chat type and is absent in personal dialogue ||
|| **CHAT_COLOR**
[`string`](../../../data-types.md) | Color scheme of the chat. This parameter depends on the chat type and is absent in personal dialogue ||
|| **CHAT_ENTITY_TYPE**
[`string`](../../../data-types.md) | Type of the object to which the chat is linked. This parameter depends on the chat type and is absent in personal dialogue ||
|| **CHAT_ENTITY_ID**
[`string`](../../../data-types.md) | Identifier of the object to which the chat is linked. This parameter depends on the chat type and is absent in personal dialogue ||
|| **CHAT_ENTITY_DATA_1**
[`string`](../../../data-types.md) | Additional data of the chat object — field 1. This parameter depends on the chat type and is absent in personal dialogue ||
|| **CHAT_ENTITY_DATA_2**
[`string`](../../../data-types.md) | Additional data of the chat object — field 2. This parameter depends on the chat type and is absent in personal dialogue ||
|| **CHAT_ENTITY_DATA_3**
[`string`](../../../data-types.md) | Additional data of the chat object — field 3. This parameter depends on the chat type and is absent in personal dialogue ||
|| **CHAT_EXTRANET**
[`string`](../../../data-types.md) | Indicator of an extranet chat: `Y` or `N`. This parameter depends on the chat type and is absent in personal dialogue ||
|| **CHAT_PREV_MESSAGE_ID**
[`integer`](../../../data-types.md) | Identifier of the previous message in the chat. This parameter depends on the chat type and is absent in personal dialogue ||
|| **CHAT_CAN_POST**
[`string`](../../../data-types.md) | Rights of the current user to send messages in the chat. This parameter depends on the chat type and is absent in personal dialogue ||
|| **RID**
[`integer`](../../../data-types.md) | Identifier of the user's connection record with the chat, system parameter ||
|| **IS_MANAGER**
[`string`](../../../data-types.md) | Indicator of the manager role: `Y` or `N` ||
|| **PUSH**
[`string`](../../../data-types.md) | Indicator of sending a push notification: `Y` or `N` ||
|| **PUSH_IMPORTANT**
[`string`](../../../data-types.md) | Indicator of an important push notification: `Y` or `N` ||
|| **RECENT_SKIP_AUTHOR**
[`string`](../../../data-types.md) | Indicator of excluding the author from the Recent Chats list: `Y` or `N` ||
|| **CONVERT**
[`string`](../../../data-types.md) | Indicator of message formatting conversion: `Y` or `N` ||
|| **SKIP_COMMAND**
[`string`](../../../data-types.md) | Skip processing commands in the message: `Y` or `N` ||
|| **SKIP_COUNTER_INCREMENTS**
[`string`](../../../data-types.md) | Do not increase unread counters: `Y` or `N` ||
|| **SILENT_CONNECTOR**
[`string`](../../../data-types.md) | Send a message to the connector without notification: `Y` or `N` ||
|| **SKIP_CONNECTOR**
[`string`](../../../data-types.md) | Do not send the message to external connectors: `Y` or `N` ||
|| **IMPORTANT_CONNECTOR**
[`string`](../../../data-types.md) | Indicator of an important message for the connector: `Y` or `N` ||
|| **NO_SESSION_OL**
[`string`](../../../data-types.md) | Do not create an open line session: `Y` or `N` ||
|| **FAKE_RELATION**
[`integer`](../../../data-types.md) | System value of the user's relation with the chat ||
|| **SKIP_URL_INDEX**
[`string`](../../../data-types.md) | Do not index links from the message: `Y` or `N` ||
|| **COMMAND_CONTEXT**
[`string`](../../../data-types.md) | Command input context ||
|| **CHAT_USER_COUNT**
[`integer`](../../../data-types.md) | Number of participants in the chat. This parameter depends on the chat type and is absent in personal dialogue ||
|| **MENTIONED_LIST**
[`object`](../../../data-types.md) | Mentions of users in the message. This parameter depends on the chat type and is absent in personal dialogue

The structure is described [below](#mentioned-list) ||
|| **PLATFORM_CONTEXT**
[`string`](../../../data-types.md) | Platform from which the message was sent ||
|| **MESSAGE_ORIGINAL**
[`string`](../../../data-types.md) | Original text of the message with BB code. This parameter depends on the chat type and is absent in personal dialogue ||
|| **MESSAGE_ID**
[`integer`](../../../data-types.md) | Identifier of the message ||
|| **LANGUAGE**
[`string`](../../../data-types.md) | Default language of Bitrix24 ||
|#

#### Parameter MENTIONED_LIST {#mentioned-list}

#|
|| **Parameter**
`type` | **Description** ||
|| **{USER_ID}**
[`integer`](../../../data-types.md) | Identifier of the user or bot mentioned in the message ||
|#

### Parameter USER {#user}

#|
|| **Parameter**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the user ||
|| **NAME**
[`string`](../../../data-types.md) | Full name of the user ||
|| **FIRST_NAME**
[`string`](../../../data-types.md) | First name of the user ||
|| **LAST_NAME**
[`string`](../../../data-types.md) | Last name of the user ||
|| **WORK_POSITION**
[`string`](../../../data-types.md) | Job position of the user ||
|| **GENDER**
[`string`](../../../data-types.md) | Gender of the user: `M` or `F` ||
|| **IS_BOT**
[`string`](../../../data-types.md) | Indicator of a bot user: `Y` or `N` ||
|| **IS_CONNECTOR**
[`string`](../../../data-types.md) | Indicator of a connector user: `Y` or `N` ||
|| **IS_NETWORK**
[`string`](../../../data-types.md) | Indicator of an external network user: `Y` or `N` ||
|| **IS_EXTRANET**
[`string`](../../../data-types.md) | Indicator of an extranet user: `Y` or `N` ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./on-imbot-message-update.md)
- [{#T}](./on-imbot-message-delete.md)
- [{#T}](../imbot-message-add.md)
- [{#T}](../imbot-message-update.md)
- [{#T}](../imbot-message-delete.md)