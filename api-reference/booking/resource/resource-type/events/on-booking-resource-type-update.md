# Event on updating resource type onBookingResourceTypeUpdate

> Scope: [`booking`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `ONBOOKINGRESOURCETYPEUPDATE` will trigger when the resource type is updated using the [booking.v1.resourcetype.update](../booking-v1-resourcetype-update.md) method.

## What the handler receives

Data is sent as a POST request {.b24-info}

```json
{
    "event": "ONBOOKINGRESOURCETYPEUPDATE",
    "event_handler_id": "2",
    "data": {
        "ID": "9"
    },
    "ts": "1751286136",
    "auth": {
        "access_token": "s6p6eclrvim6da22ft9ch94ekreb52lv",
        "expires_in": "3600",
        "scope": "booking",
        "domain": "booking.ops.bx",
        "server_endpoint": "https://oauth.bitrix.info/rest/",
        "status": "L",
        "client_endpoint": "http://booking.ops.bx/rest/",
        "member_id": "60133c09d1f5d0fd6d7884a11fad4585",
        "refresh_token": "4s386p3q0tr8dy89xvmt96234v3dljg8",
        "application_token": "tyb8wpqf7lwi471nsiv9yr1eybkafqcq"
    }
}
```

#|
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../../data-types.md) | Symbolic event code.

In this case — `ONBOOKINGRESOURCETYPEUPDATE` ||
|| **event_handler_id**
[`integer`](../../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../../data-types.md) | Object containing information about the updated resource type.

Contains the key `ID` ||
|| **data.ID**
[`integer`](../../../../data-types.md) | Identifier of the updated resource type ||
|| **ts**
[`timestamp`](../../../../data-types.md) | Date and time the event was sent from the [event queue](../../../../events/index.md) ||
|| **auth**
[`object`](../../../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../../../_includes/auth-params-in-events.md) %}

## Continue exploring

- [{#T}](../../../../events/index.md)
- [{#T}](../../../../events/event-bind.md)
- [{#T}](./on-booking-resource-type-add.md)
- [{#T}](./on-booking-resource-type-delete.md)