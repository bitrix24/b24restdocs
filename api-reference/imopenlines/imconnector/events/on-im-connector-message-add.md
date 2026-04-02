# OnImConnectorMessageAdd Event

> Scope: [`imconnector`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnImConnectorMessageAdd` event is triggered when a message is sent from an open line in Bitrix24 to an external system channel via a custom connector.

The event does not trigger:
- when receiving an incoming message from an external system channel
- for built-in connectors

After processing the event, call the [imconnector.send.status.delivery](../imconnector-send-status-delivery.md) method to mark the message as delivered in the external system channel.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONIMCONNECTORMESSAGEADD",
    "event_handler_id": 555,
    "data": {
        "CONNECTOR": "myconnector",
        "LINE": 107,
        "MESSAGES": [
            {
                "im": {
                    "chat_id": 1807,
                    "message_id": 86497
                },
                "message": {
                    "user_id": 27,
                    "text": "[b]Samantha Johnson:[/b] [br]Good afternoon!"
                },
                "chat": {
                    "id": "channel-123"
                }
            }
        ]
    },
    "ts": 1773759161,
    "auth": {
        "access_token": "c978b990071b0008440001b0895f697a",
        "expires": 1773762761,
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

In this case - `ONIMCONNECTORMESSAGEADD` ||
|| **event_handler_id**
[`integer`](../../../data-types.md) | Event handler identifier ||
|| **data**
[`object`](../../../data-types.md) | Object with event parameters.

The structure is described [below](#data) ||
|| **ts**
[`timestamp`](../../../data-types.md) | Date and time of the event sent from the [event queue](../../../events/index.md) ||
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
[`object[]`](../../../data-types.md) | Array of messages.

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
[`object`](../../../data-types.md) | Object with message text data.

The structure is described [below](#message) ||
|| **chat**
[`object`](../../../data-types.md) | Chat data in the external system.

The structure is described [below](#chat) ||
|#

### im Parameter {#im}

#|
|| **Parameter**
`type` | **Description** ||
|| **chat_id**
[`integer`](../../../data-types.md) | Identifier of the open line chat in Bitrix24 ||
|| **message_id**
[`integer`](../../../data-types.md) | Identifier of the message in Bitrix24 ||
|#

### message Parameter {#message}

#|
|| **Parameter**
`type` | **Description** ||
|| **user_id**
[`integer`](../../../data-types.md) | Identifier of the user on whose behalf the message was created ||
|| **text**
[`string`](../../../data-types.md) | Message text in the format provided in the event ||
|#

### chat Parameter {#chat}

#|
|| **Parameter**
`type` | **Description** ||
|| **id**
[`string`](../../../data-types.md) | Identifier of the chat in the external system ||
|#

### auth Parameter {#auth}

{% include notitle [Table with keys in the auth array](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](./on-im-connector-dialog-start.md)
- [{#T}](./on-im-connector-message-update.md)
- [{#T}](./on-im-connector-message-delete.md)
- [{#T}](./on-im-connector-dialog-finish.md)
- [{#T}](./on-im-connector-status-delete.md)
- [{#T}](./on-im-connector-line-delete.md)