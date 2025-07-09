# Event on Deleting a Record from the Waitlist onBookingWaitListItemDelete

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `ONBOOKINGWAITLISTITEMDELETE` will trigger when a record is deleted from the waitlist either manually or via the [booking.v1.waitlist.delete](../booking-v1-waitlist-delete.md) method.

## What the Handler Receives

Data is sent as a POST request {.b24-info}

```json
{
    "event": "ONBOOKINGWAITLISTITEMDELETE",
    "event_handler_id": "12",
    "data": {
        "ID": "1"
    },
    "ts": "1751285534",
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

In this case — `ONBOOKINGWAITLISTITEMDELETE` ||
|| **event_handler_id**
[`integer`](../../../data-types.md) | Identifier of the event handler ||
|| **data**
[`object`](../../../data-types.md) | Object containing information about the deleted record in the waitlist.

Contains the key `ID` ||
|| **data.ID**
[`integer`](../../../data-types.md) | Identifier of the deleted record in the waitlist ||
|| **ts**
[`timestamp`](../../../data-types.md) | Date and time the event was sent from the [event queue](../../../events/index.md) ||
|| **auth**
[`object`](../../../data-types.md) | Object containing authorization parameters and data about the account where the event occurred.

The structure is described [below](#auth) ||
|#

### Parameter auth {#auth}

{% include notitle [Table with keys in the auth array](../../../../_includes/auth-params-in-events.md) %}

## Continue Exploring

- [{#T}](../../../events/index.md)
- [{#T}](../../../events/event-bind.md)
- [{#T}](./on-booking-waitlistitem-add.md)
- [{#T}](./on-booking-waitlistitem-update.md)