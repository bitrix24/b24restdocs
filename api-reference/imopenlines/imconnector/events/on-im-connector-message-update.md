# OnImConnectorMessageUpdate Event

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`imconnector`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnImConnectorMessageUpdate` event is triggered when a message is modified in an open line of Bitrix24, and the change is sent to the external system channel via a custom connector.

The event does not trigger:
- for built-in connectors
- when a message sent from the external system channel is modified

After processing the event, call the [imconnector.send.status.delivery](../imconnector-send-status-delivery.md) method to mark the message as successfully updated in the external system channel.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONIMCONNECTORMESSAGEUPDATE",
    "event_handler_id": 557,
    "data": {
        "CONNECTOR": "myconnector",
        "LINE": 107,
        "MESSAGES": [
            {
                "im": {
                    "chat_id": 1807,
                    "message_id": 86501
                },
                "message": {
                    "id": ["ext-msg-005"],
                    "text": "[b]Samantha Johnson:[/b][br] What questions do you have?"
                },
                "chat": {
                    "id": "channel-123"
                }
            }
        ]
    },
    "ts": 1773760030,
    "auth": {
        "access_token": "2e7cb0071b04400007eb2a255d2a542d6d0ca03ceb",
        "expires": 1773763630,
        "expires_in": 3600,
        "scope": "imconnector, imopenlines",
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "F",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "member_id": "bac1cd5c8940947a75e0d71b1a84e348",
        "user_id": 27,
        "application_token": "831c76b092f9f135d9b6b36c3a720757"
    }
}
```

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../data-types.md) | Symbolic event code.

In this case - `ONIMCONNECTORMESSAGEUPDATE` ||
|| **event_handler_id**
[`integer`](../../../data-types.md) | Event handler identifier ||
|| **data**
[`object`](../../../data-types.md) | Object with parameters of the outgoing message change event.

The structure is described [below](#data) ||
|| **ts**
[`timestamp`](../../../data-types.md) | Date and time the event was sent from the [event queue](../../../events/index.md) ||
|| **auth**
[`object`](../../../data-types.md) | Object with authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Data Parameter {#data}

#|
|| **Parameter**
`type` | **Description** ||
|| **CONNECTOR**
[`string`](../../../data-types.md) | Connector identifier ||
|| **LINE**
[`integer`](../../../data-types.md) | Open line identifier ||
|| **MESSAGES**
[`object[]`](../../../data-types.md) | Array of messages that were modified in Bitrix24 and sent to the external system channel.

The structure of the array element is described [below](#messages-item) ||
|#

### MESSAGES Array Element {#messages-item}

#|
|| **Parameter**
`type` | **Description** ||
|| **im**
[`object`](../../../data-types.md) | Object with message identifiers in Bitrix24.

The structure is described [below](#im) ||
|| **message**
[`object`](../../../data-types.md) | Object with data of the modified message for the external system channel.

The structure is described [below](#message) ||
|| **chat**
[`object`](../../../data-types.md) | Object with the chat identifier in the external system channel.

The structure is described [below](#chat) ||
|#

### im Parameter {#im}

#|
|| **Parameter**
`type` | **Description** ||
|| **chat_id**
[`integer`](../../../data-types.md) | Chat identifier in Bitrix24 ||
|| **message_id**
[`integer`](../../../data-types.md) | Identifier of the modified message in Bitrix24 ||
|#

### message Parameter {#message}

#|
|| **Parameter**
`type` | **Description** ||
|| **id**
[`string[]`](../../../data-types.md) | Array of identifiers of the modified message in the external system channel ||
|| **text**
[`string`](../../../data-types.md) | New text of the message in the format provided in the event ||
|#

### chat Parameter {#chat}

#|
|| **Parameter**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Identifier of the chat in the external system channel ||
|#

### auth Parameter {#auth}

{% include notitle [Table with keys in the auth array](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](./on-im-connector-message-add.md)
- [{#T}](./on-im-connector-dialog-start.md)
- [{#T}](./on-im-connector-message-delete.md)
- [{#T}](./on-im-connector-dialog-finish.md)
- [{#T}](./on-im-connector-status-delete.md)
- [{#T}](./on-im-connector-line-delete.md)