# When Creating an Open Channel Session OnSessionStart

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnSessionStart` event is triggered when a new Open Channel session is created: when a customer writes to the channel through any communication channel, including the live chat, or when a session is started with the [imopenlines.session.start](../sessions/imopenlines-session-start.md) method.

[Subscribing](../../../events/event-bind.md) to the event can only be done through the application. The event arrives for all open channel communication channels, not only for the application connector. To handle only your own sessions, check the connector code in `connector.connector_id`.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONSESSIONSTART",
    "event_handler_id": 145,
    "data": {
        "DATA": {
            "connector": {
                "connector_id": "my_connector",
                "line_id": 1,
                "chat_id": 17,
                "user_id": 10
            },
            "chat": {
                "chat_id": 17
            },
            "line": {
                "id": 1,
                "name": "Open channel"
            }
        }
    },
    "ts": 1714649632,
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": 3600,
        "scope": "imopenlines",
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "F",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "member_id": "a223c6b3710f85df22e9377d6c4f7553",
        "refresh_token": "4s386p3q0tr8dy89xvmt96234v3dljg8",
        "application_token": "51856fefc120afa4b628cc82d3935cce"
    }
}
```

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **event***
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `ONSESSIONSTART` ||
|| **event_handler_id***
[`integer`](../../../data-types.md) | Event handler ID ||
|| **data***
[`object`](../../../data-types.md) | An object with data of the created session.

Contains a single key, `DATA` ||
|| **data.DATA***
[`object`](../../../data-types.md) | An object with information about the connector, chat, and open channel.

The structure is described [below](#data) ||
|| **ts***
[`timestamp`](../../../data-types.md) | Date and time of the event sent from the [event queue](../../../events/index.md) ||
|| **auth***
[`object`](../../../data-types.md) | Object containing authorization parameters and information about the Bitrix24 account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter DATA {#data}

#|
|| **Parameter**
`type` | **Description** ||
|| **connector***
[`object`](../../../data-types.md) | An object with information about the communication channel of the session.

The structure is described [below](#connector) ||
|| **chat***
[`object`](../../../data-types.md) | An object with information about the session chat.

The structure is described [below](#chat) ||
|| **line***
[`object`](../../../data-types.md) | An object with information about the open channel.

The structure is described [below](#line) ||
|#

#### Parameter connector {#connector}

#|
|| **Parameter**
`type` | **Description** ||
|| **connector_id***
[`string`](../../../data-types.md) | Code of the connector through which the customer writes to the channel. For example, `livechat` for the live chat or the code of the application connector ||
|| **line_id***
[`integer`](../../../data-types.md) | Identifier of the open line ||
|| **chat_id***
[`integer`](../../../data-types.md) | Open channel chat ID in Bitrix24 ||
|| **user_id***
[`integer`](../../../data-types.md) | Customer ID in Bitrix24 ||
|#

#### Parameter chat {#chat}

#|
|| **Parameter**
`type` | **Description** ||
|| **chat_id***
[`integer`](../../../data-types.md) | Open channel chat ID in Bitrix24. Matches `connector.chat_id` ||
|#

#### Parameter line {#line}

#|
|| **Parameter**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Open channel ID. Matches `connector.line_id` ||
|| **name***
[`string`](../../../data-types.md) | Open channel name ||
|#

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../../events/index.md)
- [{#T}](../../../events/event-bind.md)
- [{#T}](./on-session-finish.md)
- [{#T}](./on-open-line-message-add.md)
- [{#T}](./on-open-line-message-update.md)
- [{#T}](./on-open-line-message-delete.md)
