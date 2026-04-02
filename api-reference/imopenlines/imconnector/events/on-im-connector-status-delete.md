# OnImConnectorStatusDelete Event for Disabling Open Channels

> Scope: [`imconnector`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnImConnectorStatusDelete` event is triggered when a client disables the connector of an external channel on a specific open channel.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONIMCONNECTORSTATUSDELETE",
    "event_handler_id": 561,
    "data": {
        "connector": "myconnector",
        "line": 107
    },
    "ts": 1773761094,
    "auth": {
        "access_token": "5680b91b0000844000785e072978228d24bd2f",
        "expires": 1773764694,
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
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `ONIMCONNECTORSTATUSDELETE` ||
|| **event_handler_id**
[`integer`](../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../data-types.md) | Object containing event data.

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
|| **connector**
[`string`](../../../data-types.md) | Identifier of the disabled connector ||
|| **line**
[`integer`](../../../data-types.md) | Identifier of the open channel for which the connector was disabled ||
|#

### Auth Parameter {#auth}

{% include notitle [Table with keys in the auth array](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](./on-im-connector-message-add.md)
- [{#T}](./on-im-connector-dialog-start.md)
- [{#T}](./on-im-connector-message-update.md)
- [{#T}](./on-im-connector-message-delete.md)
- [{#T}](./on-im-connector-dialog-finish.md)
- [{#T}](./on-im-connector-line-delete.md)