# Event on Booking Update onBookingUpdate

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `ONBOOKINGUPDATE` will trigger when a booking is updated manually or via the method [booking.v1.booking.update](../booking-v1-booking-update.md).

## What the handler receives

Data is sent as a POST request {.b24-info}

```json
{
    "event": "ONBOOKINGUPDATE",
    "event_handler_id": "8",
    "data": {
        "ID": "66"
    },
    "ts": "1751285644",
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
|| **Parameter**
`type` | **Description** ||
|| **event**
[`string`](../../../data-types.md) | Symbolic code of the event.

In this case — `ONBOOKINGUPDATE` ||
|| **event_handler_id**
[`integer`](../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../data-types.md) | Object containing information about the updated booking.

Contains the key `ID` ||
|| **data.ID**
[`integer`](../../../data-types.md) | Identifier of the updated booking ||
|| **ts**
[`timestamp`](../../../data-types.md) | Date and time the event was sent from the [event queue](../../../events/index.md) ||
|| **auth**
[`object`](../../../data-types.md) | Object containing authorization parameters and information about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../../_includes/auth-params-in-events.md) %}

## Continue exploring

- [{#T}](../../../events/index.md)
- [{#T}](../../../events/event-bind.md)
- [{#T}](./on-booking-add.md)
- [{#T}](./on-booking-delete.md)