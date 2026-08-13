# OnOpenLineMessageUpdate When a Chat Message Is Modified

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnOpenLineMessageUpdate` event is triggered when a message in the open line chat is modified.

[Subscribe](../../../events/event-bind.md) to the event can only be done through the application. Only those events intended for the [connector](../../imconnector/index.md) added by the application can be received in the handler.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONOPENLINEMESSAGEUPDATE",
    "eventId": 1,
    "data": {
        "CONNECTOR": "livechat",
        "LINE": 128,
        "DATA": [
            {
                "im": {
                    "chat_id": 1024,
                    "message_id": 2056
                },
                "message": {
                    "id": 2056
                },
                "chat": {
                    "id": 1024
                }
            }
        ]
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
|| **parameter**
`type` | **Description** ||
|| **event***
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `ONOPENLINEMESSAGEUPDATE` ||
|| **eventId***
[`integer`](../../../data-types.md) | Event identifier ||
|| **data***
[`object`](../../../data-types.md) | An object containing event data.

The structure is described [below](#data) ||
|| **ts***
[`timestamp`](../../../data-types.md) | Date and time of the event sent from the [event queue](../../../events/index.md) ||
|| **auth***
[`object`](../../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

#|
|| **parameter**
`type` | **Description** ||
|| **CONNECTOR***
[`string`](../../../data-types.md) | Connector identifier ||
|| **LINE***
[`integer`](../../../data-types.md) | Identifier of the open line ||
|| **DATA***
[`array`](../../../data-types.md) | An array of objects with data from the modified message.

The object structure is described [below](#chat-params) ||
|#

#### Array Element DATA {#chat-params}

Each array element `DATA` is an object with the following structure:

#|
|| **parameter**
`type` | **Description** ||
|| **im***
[`object`](../../../data-types.md) | An object with information about the modified message in the chat.

The structure is described [below](#im) ||
|| **message***
[`object`](../../../data-types.md) | An object with information about the message.

The structure is described [below](#message) ||
|| **chat***
[`object`](../../../data-types.md) | An object with information about the chat.

The structure is described [below](#chat) ||
|#

##### Parameter im {#im}

#|
|| **parameter**
`type` | **Description** ||
|| **chat_id***
[`integer`](../../../data-types.md) | Identifier of the chat ||
|| **message_id***
[`integer`](../../../data-types.md) | Identifier of the message ||
|#

##### Parameter message {#message}

#|
|| **parameter**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the message ||
|#

##### Parameter chat {#chat}

#|
|| **parameter**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the chat ||
|#

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../../events/index.md)
- [{#T}](../../../events/event-bind.md)
- [{#T}](./on-open-line-message-add.md)
- [{#T}](./on-open-line-message-delete.md)
- [{#T}](./on-session-start.md)
- [{#T}](./on-session-finish.md)
