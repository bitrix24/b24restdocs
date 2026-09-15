# Event onUpdating Resource onBookingResourceUpdate

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `ONBOOKINGRESOURCEUPDATE` will trigger when a resource is updated manually or via the [booking.v1.resource.update](../booking-v1-resource-update.md) method. The slot configuration methods also trigger this event — [booking.v1.resource.slots.set](../slots/booking-v1-resource-slots-set.md) and [booking.v1.resource.slots.unset](../slots/booking-v1-resource-slots-unset.md): slots are retained within the resource itself. The handler receives only the identifier — retrieve the remaining resource fields using the [booking.v1.resource.get](../booking-v1-resource-get.md) method.

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```json
{
    "event": "ONBOOKINGRESOURCEUPDATE",
    "event_handler_id": "5",
    "data": {
        "ID": "10"
    },
    "ts": "1751285842",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "booking",
        "domain": "some-domain.bitrix24.com",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "L",
        "client_endpoint": "https://some-domain.bitrix24.com/rest/",
        "member_id": "60133c09d1f5d0fd6d7884a11fad4585",
        "refresh_token": "4s386p3q0tr8dy89xvmt96234v3dljg8",
        "application_token": "tyb8wpqf7lwi471nsiv9yr1eybkafqcq"
    }
}
```

#|
|| **parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `ONBOOKINGRESOURCEUPDATE` ||
|| **event_handler_id**
[`integer`](../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../data-types.md) | Object containing information about the updated resource.

Contains the key `ID` ||
|| **data.ID**
[`integer`](../../../data-types.md) | Identifier of the updated resource ||
|| **ts**
[`timestamp`](../../../data-types.md) | Date and time of the event sent from the [event queue](../../../events/index.md) ||
|| **auth**
[`object`](../../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter auth {#auth}

{% include notitle [Auth parameters in events](../../../../_includes/auth-params-in-events.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](../../../events/index.md)
- [{#T}](../../../events/event-bind.md)
- [{#T}](./on-booking-resource-add.md)
- [{#T}](./on-booking-resource-delete.md)
