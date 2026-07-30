# When Adding a Message to the Chat OnOpenLineMessageAdd

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnOpenLineMessageAdd` event is triggered when a message is added to the open line chat.

[Subscribe](../../../events/event-bind.md) to the event can only be done through the application. Only those events intended for the [connector](../../imconnector/index.md) added by the application can be received in the handler.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONOPENLINEMESSAGEADD",
    "eventId": 1,
    "data": {
        "DATA": [
            {
                "connector": {
                    "connector_id": "livechat",
                    "line_id": 128,
                    "chat_id": 10587,
                    "user_id": 1985
                },
                "chat": {
                    "id": 10585
                },
                "message": {
                    "id": 80964,
                    "date": "",
                    "text": "hello",
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

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **parameter**
`type` | **Description** ||
|| **event***
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `ONOPENLINEMESSAGEADD` ||
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
|| **DATA***
[`array`](../../../data-types.md) | An array of objects with message data.

The object structure is described [below](#chat-params) ||
|#

#### Array Element DATA {#chat-params}

Each array element `DATA` is an object with the following structure:

#|
|| **parameter**
`type` | **Description** ||
|| **connector***
[`object`](../../../data-types.md) | An object with connector information.

The structure is described [below](#connector) ||
|| **chat***
[`object`](../../../data-types.md) | An object with chat information.

The structure is described [below](#chat) ||
|| **message***
[`object`](../../../data-types.md) | An object with message information.

The structure is described [below](#message) ||
|| **ref***
[`array`](../../../data-types.md) | An array with the tracker code `trackId` to link the message to a CRM object. In the example, it is passed as empty ||
|| **extra***
[`object`](../../../data-types.md) | An object with additional information.

The structure is described [below](#extra) ||
|#

##### Parameter connector {#connector}

#|
|| **parameter**
`type` | **Description** ||
|| **connector_id***
[`string`](../../../data-types.md) | Connector identifier ||
|| **line_id***
[`integer`](../../../data-types.md) | Identifier of the open line ||
|| **chat_id***
[`integer`](../../../data-types.md) | Identifier of the chat ||
|| **user_id***
[`integer`](../../../data-types.md) | User ID in the external system ||
|#

##### Parameter chat {#chat}

#|
|| **parameter**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the chat ||
|#

##### Parameter message {#message}

#|
|| **parameter**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the message ||
|| **date***
[`string`](../../../data-types.md) | Date and time the message was added ||
|| **text***
[`string`](../../../data-types.md) | Text of the message ||
|| **files***
[`array`](../../../data-types.md) | Message files ||
|| **attach***
[`string`](../../../data-types.md) | Attached data ||
|| **system***
[`string`](../../../data-types.md) | A flag indicating that the message is a system message: `Y` — yes, `N` — no ||
|| **user_id***
[`integer`](../../../data-types.md) | User identifier ||
|#

##### Parameter extra {#extra}

#|
|| **parameter**
`type` | **Description** ||
|| **EXTRA_URL***
[`string`](../../../data-types.md) | External link for Bitrix24.Network ||
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
