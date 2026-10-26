# OnShipmentDeleted Event

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The `OnShipmentDeleted` event occurs when a shipment is deleted from the database manually, with the [sale.shipment.delete](../shipment/sale-shipment-delete.md) method, or together with the order.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONSHIPMENTDELETED",
    "event_handler_id": 6,
    "data": {
        "FIELDS": {
            "ID": 300
        }
    },
    "ts": 1714649632,
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": 3600,
        "scope": "sale",
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

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../data-types.md) | Symbolic code of the event.

In this case — `ONSHIPMENTDELETED` ||
|| **event_handler_id***
[`integer`](../../data-types.md) | Identifier of the event handler ||
|| **data***
[`object`](../../data-types.md) | An object containing information about a remote shipment.

The structure is described [below](#data) ||
|| **ts***
[`timestamp`](../../data-types.md) | Date and time of the event sent from the [event queue](../../events/index.md) ||
|| **auth***
[`object`](../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter data {#data}

#|
|| **Name**
`type` | **Description** ||
|| **FIELDS***
[`object`](../../data-types.md) | An object with property `ID`, containing the shipment identifier.

The structure is described [below](#fields) ||
|#

#### Parameter FIELDS {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`sale_order_shipment.id`](../data-types.md#sale_order_shipment) | Identifier of the deleted shipment. The shipment has already been deleted, so the [sale.shipment.get](../shipment/sale-shipment-get.md) method will not return its data. If the handler needs the shipment fields, store them in advance, for example, on the [OnShipmentEntitySaved](./on-shipment-entity-saved.md) event ||
|#

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](../../events/index.md)
- [{#T}](../../events/event-bind.md)
- [{#T}](./index.md)
- [{#T}](./on-shipment-entity-saved.md)
- [{#T}](./on-order-deleted.md)
