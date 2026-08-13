# OnImConnectorDialogFinish Event

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imconnector`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnImConnectorDialogFinish` event is triggered when a conversation in an Open Channel is closed.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONIMCONNECTORDIALOGFINISH",
    "eventId": 1,
    "data": {
        "CONNECTOR": "newcustomconnector",
        "LINE": "105",
        "DATA": [
            {
                "connector": {
                    "connector_id": "newcustomconnector",
                    "line_id": 105,
                    "chat_id": 8,
                    "user_id": 0
                },
                "session": {
                    "id": 3282,
                    "closed": "Y",
                    "parent_id": 0,
                    "close_term": 10
                },
                "chat": {
                    "id": 8
                },
                "user": {
                    "id": 0
                }
            }
        ]
    },
    "ts": 1714649632,
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": 3600,
        "scope": "imconnector",
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

In this case — `ONIMCONNECTORDIALOGFINISH` ||
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
|| **Parameter**
`type` | **Description** ||
|| **CONNECTOR***
[`string`](../../../data-types.md) | Connector identifier ||
|| **LINE***
[`string`](../../../data-types.md) | Identifier of the open line ||
|| **DATA***
[`array`](../../../data-types.md) | An array of objects with dialogue data.

The object structure is described [below](#dialog-params) ||
|#

#### Array Element DATA {#dialog-params}

Each array element `DATA` is an object with the following structure:

#|
|| **Parameter**
`type` | **Description** ||
|| **connector***
[`object`](../../../data-types.md) | An object with connector information.

The structure is described [below](#connector) ||
|| **session***
[`object`](../../../data-types.md) | An object with session information.

The structure is described [below](#session) ||
|| **chat***
[`object`](../../../data-types.md) | An object with chat information.

The structure is described [below](#chat) ||
|| **user***
[`object`](../../../data-types.md) | An object with user information.

The structure is described [below](#user) ||
|#

##### Parameter connector {#connector}

#|
|| **Parameter**
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

##### Parameter session {#session}

#|
|| **Parameter**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the session ||
|| **closed***
[`string`](../../../data-types.md) | A flag indicating the dialogue is closed. `Y` — dialogue is closed ||
|| **parent_id***
[`integer`](../../../data-types.md) | Previous session identifier ||
|| **close_term***
[`integer`](../../../data-types.md) | Number of minutes until session closure ||
|#

##### Parameter chat {#chat}

#|
|| **Parameter**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the chat ||
|#

##### Parameter user {#user}

#|
|| **Parameter**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | User ID in the external system ||
|#

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../../events/index.md)
- [{#T}](../../../events/event-bind.md)
- [{#T}](./on-im-connector-dialog-start.md)
- [{#T}](./on-im-connector-message-add.md)
- [{#T}](./on-im-connector-message-update.md)
- [{#T}](./on-im-connector-message-delete.md)
- [{#T}](./on-im-connector-status-delete.md)
- [{#T}](./on-im-connector-line-delete.md)
