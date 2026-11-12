# When Adding a Message to an Open Channel Chat OnOpenLineMessageAdd

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnOpenLineMessageAdd` event is triggered when a message is added to an open channel chat:

- a customer has written a message through a connector or the live chat
- Bitrix24 has passed an operator or system message to the connector, including a message sent with the [imopenlines.crm.message.add](../messages/imopenlines-crm-message-add.md) method

For operator messages in the live chat, the event is not sent.

[Subscribing](../../../events/event-bind.md) to the event can only be done through the application. The event arrives for all open channel communication channels, not only for the application connector. To handle only your own messages, check the connector code in `DATA.connector.connector_id`.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

Data content in `data` depends on who wrote the message.

**Customer message.** `data` contains only the `DATA` key:

```json
{
    "event": "ONOPENLINEMESSAGEADD",
    "event_handler_id": 145,
    "data": {
        "DATA": {
            "connector": {
                "connector_id": "livechat",
                "line_id": 1,
                "chat_id": 10585,
                "user_id": 1985
            },
            "chat": {
                "id": 10585
            },
            "message": {
                "id": 80964,
                "date": "",
                "text": "Hello, I need some advice",
                "files": [],
                "attach": "",
                "system": "N",
                "user_id": 1985
            },
            "ref": [],
            "extra": {
                "EXTRA_URL": ""
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

**Operator or system message.** Besides `DATA`, `data` contains the `CONNECTOR` and `LINE` keys:

```json
{
    "event": "ONOPENLINEMESSAGEADD",
    "event_handler_id": 145,
    "data": {
        "CONNECTOR": "my_connector",
        "LINE": 1,
        "DATA": {
            "connector": {
                "connector_id": "my_connector",
                "line_id": 1,
                "user_id": 1985,
                "chat_id": "ext-chat-42"
            },
            "message": {
                "id": 80970,
                "chat_id": 10587,
                "user_id": 1,
                "text": "Good afternoon! How can I help you?",
                "files": [],
                "attachments": [],
                "params": [],
                "system": "N"
            },
            "no_session": "N"
        }
    },
    "ts": 1714649640,
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
|| **parameter**
`type` | **Description** ||
|| **event***
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `ONOPENLINEMESSAGEADD` ||
|| **event_handler_id***
[`integer`](../../../data-types.md) | Event handler ID ||
|| **data***
[`object`](../../../data-types.md) | An object with data of the added message.

The structure is described [below](#data) ||
|| **ts***
[`timestamp`](../../../data-types.md) | Date and time of the event sent from the [event queue](../../../events/index.md) ||
|| **auth***
[`object`](../../../data-types.md) | Object containing authorization parameters and information about the Bitrix24 account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

#|
|| **parameter**
`type` | **Description** ||
|| **CONNECTOR**
[`string`](../../../data-types.md) | Code of the connector to which Bitrix24 passed the message. Arrives only for an operator or system message ||
|| **LINE**
[`integer`](../../../data-types.md) | Open channel ID. Arrives only for an operator or system message ||
|| **DATA***
[`object`](../../../data-types.md) | An object with message data.

For a customer message, the structure is described [below](#client-data); for an operator or system message, see [below](#operator-data) ||
|#

### DATA for a Customer Message {#client-data}

#|
|| **parameter**
`type` | **Description** ||
|| **connector***
[`object`](../../../data-types.md) | An object with connector information.

The structure is described [below](#client-connector) ||
|| **chat***
[`object`](../../../data-types.md) | An object with information about the chat passed by the connector.

The structure is described [below](#client-chat) ||
|| **message***
[`object`](../../../data-types.md) | An object with information about the customer message.

The structure is described [below](#client-message) ||
|| **ref***
[`array`](../../../data-types.md) | An array with the tracker code `trackId` to link the message to a CRM object. If the connector has not passed it, an empty array arrives ||
|| **extra***
[`object`](../../../data-types.md) | An object with additional information.

The structure is described [below](#client-extra) ||
|#

#### Parameter connector {#client-connector}

#|
|| **parameter**
`type` | **Description** ||
|| **connector_id***
[`string`](../../../data-types.md) | Connector code. For the live chat — `livechat` ||
|| **line_id***
[`integer`](../../../data-types.md) | Identifier of the open line ||
|| **chat_id***
[`string`](../../../data-types.md) | ID of the chat passed by the connector. For an external communication channel, this is the chat ID in the external system; for the live chat, it is the live chat ID in Bitrix24 ||
|| **user_id***
[`integer`](../../../data-types.md) | Customer ID in Bitrix24 ||
|#

#### Parameter chat {#client-chat}

The `chat` object contains the chat fields as the connector passed them, for example, with the [imconnector.send.messages](../../imconnector/imconnector-send-messages.md) method.

#|
|| **parameter**
`type` | **Description** ||
|| **id***
[`string`](../../../data-types.md) | Chat ID. Matches `connector.chat_id` ||
|#

#### Parameter message {#client-message}

The `message` object contains the message fields as the connector passed them. For the live chat, the set of fields is shown in the example.

#|
|| **parameter**
`type` | **Description** ||
|| **id***
[`string`](../../../data-types.md) | Message ID. For an external communication channel, this is the message ID in the external system ||
|| **date**
[`string`](../../../data-types.md) | Date and time of the message. For the live chat, an empty string arrives ||
|| **text**
[`string`](../../../data-types.md) | Text of the message ||
|| **files**
[`array`](../../../data-types.md) | Message files ||
|| **attach**
[`string`](../../../data-types.md) | Message attachment. Arrives for the live chat ||
|| **system**
[`string`](../../../data-types.md) | A flag indicating that the message is a system message: `Y` — yes, `N` — no. Arrives for the live chat ||
|| **user_id***
[`integer`](../../../data-types.md) | Customer ID in Bitrix24 ||
|#

#### Parameter extra {#client-extra}

#|
|| **parameter**
`type` | **Description** ||
|| **EXTRA_URL**
[`string`](../../../data-types.md) | Website address from the customer profile. Arrives for the live chat ||
|#

### DATA for an Operator or System Message {#operator-data}

#|
|| **parameter**
`type` | **Description** ||
|| **connector***
[`object`](../../../data-types.md) | An object with information about the connector and the customer chat.

The structure is described [below](#operator-connector) ||
|| **message***
[`object`](../../../data-types.md) | An object with information about the message in Bitrix24.

The structure is described [below](#operator-message) ||
|| **no_session***
[`string`](../../../data-types.md) | A flag indicating that the message was sent without being linked to a session: `Y` — yes, `N` — no ||
|#

#### Parameter connector {#operator-connector}

#|
|| **parameter**
`type` | **Description** ||
|| **connector_id***
[`string`](../../../data-types.md) | Connector code ||
|| **line_id***
[`integer`](../../../data-types.md) | Identifier of the open line ||
|| **user_id***
[`integer`](../../../data-types.md) | Customer ID in Bitrix24 ||
|| **chat_id***
[`string`](../../../data-types.md) | Chat ID in the external system ||
|#

#### Parameter message {#operator-message}

#|
|| **parameter**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Message ID in Bitrix24. Pass it to the [imconnector.send.status.delivery](../../imconnector/imconnector-send-status-delivery.md) method to confirm delivery ||
|| **chat_id***
[`integer`](../../../data-types.md) | Open channel chat ID in Bitrix24 ||
|| **user_id***
[`integer`](../../../data-types.md) | ID of the message author in Bitrix24 ||
|| **text***
[`string`](../../../data-types.md) | Text of the message ||
|| **files***
[`array`](../../../data-types.md) | Message files ||
|| **attachments***
[`array`](../../../data-types.md) | Message attachments ||
|| **params***
[`array`](../../../data-types.md) | Additional message parameters ||
|| **system***
[`string`](../../../data-types.md) | A flag indicating that the message is a system message: `Y` — yes, `N` — no ||
|#

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../../events/index.md)
- [{#T}](../../../events/event-bind.md)
- [{#T}](./on-open-line-message-update.md)
- [{#T}](./on-open-line-message-delete.md)
- [{#T}](./on-session-start.md)
- [{#T}](./on-session-finish.md)
