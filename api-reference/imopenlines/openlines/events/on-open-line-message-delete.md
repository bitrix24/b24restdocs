# When Deleting a Message in an Open Channel Chat OnOpenLineMessageDelete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnOpenLineMessageDelete` event is triggered when a message in an open channel chat is deleted:

- a customer has deleted a message through a connector, for example, the connector has called the [imconnector.delete.messages](../../imconnector/imconnector-delete-messages.md) method
- an operator has deleted a message that Bitrix24 has already passed to the external communication channel

For an operator message, the event arrives only if the connector has confirmed its delivery with the [imconnector.send.status.delivery](../../imconnector/imconnector-send-status-delivery.md) method. Without confirmation, Bitrix24 does not know the message ID in the external system and does not send the event. For the live chat, the event about deleting an operator message is not sent.

[Subscribing](../../../events/event-bind.md) to the event can only be done through the application. The event arrives for all open channel communication channels, not only for the application connector. Check the connector code in `CONNECTOR` or `DATA.connector.connector_id`.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

The content of `data` depends on who deleted the message.

**Customer message.** `data` contains only the `DATA` key. Its content is the same as for a customer message in the [OnOpenLineMessageAdd](./on-open-line-message-add.md#client-data) event:

```json
{
    "event": "ONOPENLINEMESSAGEDELETE",
    "event_handler_id": 147,
    "data": {
        "DATA": {
            "connector": {
                "connector_id": "my_connector",
                "line_id": 1,
                "chat_id": "ext-chat-42",
                "user_id": 1985
            },
            "chat": {
                "id": "ext-chat-42"
            },
            "message": {
                "id": "ext-msg-501",
                "user_id": 1985
            },
            "ref": [],
            "extra": []
        }
    },
    "ts": 1714649702,
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

**Operator message.** Besides `DATA`, `data` contains the `CONNECTOR` and `LINE` keys. Here `DATA` is an array with one element for each message ID in the external system:

```json
{
    "event": "ONOPENLINEMESSAGEDELETE",
    "event_handler_id": 147,
    "data": {
        "CONNECTOR": "my_connector",
        "LINE": 1,
        "DATA": [
            {
                "im": {
                    "chat_id": 10587,
                    "message_id": 80970
                },
                "message": {
                    "id": "ext-msg-777"
                },
                "chat": {
                    "id": "ext-chat-42"
                }
            }
        ]
    },
    "ts": 1714649710,
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

In this case — `ONOPENLINEMESSAGEDELETE` ||
|| **event_handler_id***
[`integer`](../../../data-types.md) | Event handler ID ||
|| **data***
[`object`](../../../data-types.md) | An object with data of the deleted message.

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
[`string`](../../../data-types.md) | Connector code. Arrives only for an operator message ||
|| **LINE**
[`integer`](../../../data-types.md) | Open channel ID. Arrives only for an operator message ||
|| **DATA***
[`object`](../../../data-types.md) \| [`array`](../../../data-types.md) | Message data.

For a customer message, it is an object; its structure is described in the [OnOpenLineMessageAdd](./on-open-line-message-add.md#client-data) event. For an operator message, it is an array of objects; the element structure is described [below](#operator-data) ||
|#

### DATA Element for an Operator Message {#operator-data}

#|
|| **parameter**
`type` | **Description** ||
|| **im***
[`object`](../../../data-types.md) | An object with information about the message in Bitrix24.

The structure is described [below](#im) ||
|| **message***
[`object`](../../../data-types.md) | An object with the message ID in the external system.

The structure is described [below](#message) ||
|| **chat***
[`object`](../../../data-types.md) | An object with information about the chat in the external system.

The structure is described [below](#chat) ||
|#

#### Parameter im {#im}

#|
|| **parameter**
`type` | **Description** ||
|| **chat_id***
[`integer`](../../../data-types.md) | Open channel chat ID in Bitrix24 ||
|| **message_id***
[`integer`](../../../data-types.md) | Message ID in Bitrix24 ||
|#

#### Parameter message {#message}

#|
|| **parameter**
`type` | **Description** ||
|| **id***
[`string`](../../../data-types.md) | Message ID in the external system that the connector passed when confirming delivery ||
|#

#### Parameter chat {#chat}

#|
|| **parameter**
`type` | **Description** ||
|| **id***
[`string`](../../../data-types.md) | Chat ID in the external system ||
|#

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../../events/index.md)
- [{#T}](../../../events/event-bind.md)
- [{#T}](./on-open-line-message-add.md)
- [{#T}](./on-open-line-message-update.md)
- [{#T}](./on-session-start.md)
- [{#T}](./on-session-finish.md)
