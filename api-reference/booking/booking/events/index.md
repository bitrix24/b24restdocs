# Overview of Events When Working with Bookings

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes in near real-time: receiving notifications about the creation, updating, and deletion of bookings.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

Waitlist record events are described in the article [{#T}](../../waitlist/events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to events in the section through:

- [outbound webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Events in Bitrix24](../../../events/test-handler.md).

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

## What the Handler Receives

All three booking events pass only the booking identifier in the `data` object. The identifier is passed as a string:

```json
{
    "event": "ONBOOKINGUPDATE",
    "event_handler_id": "8",
    "data": {
        "ID": "66"
    },
    "ts": "1751285644"
}
```

The event contains neither field values nor a list of changes. To find out what has changed, retrieve the booking using the [booking.v1.booking.get](../booking-v1-booking-get.md) method and compare the data with your copy. After the `onBookingDelete` event, this method returns the `1021` error with the description `Booking not found`.

**Moving between a booking and the waitlist.** The [booking.v1.booking.createfromwaitlist](../booking-v1-booking-createfromwaitlist.md) method creates a booking and deletes the original waitlist record: `onBookingAdd` arrives first, followed by `onBookingWaitListItemDelete`. The [booking.v1.waitlist.createfrombooking](../../waitlist/booking-v1-waitlist-createfrombooking.md) method works the other way: `onBookingWaitListItemAdd` arrives first, followed by `onBookingDelete`.

The request also contains the `auth` key. It holds the authorization parameters and account data; its structure is described on the event page, for example [onBookingAdd](./on-booking-add.md#auth).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [onBookingAdd](./on-booking-add.md) | When a booking is created manually or via the methods [booking.v1.booking.add](../booking-v1-booking-add.md), [booking.v1.booking.createfromwaitlist](../booking-v1-booking-createfromwaitlist.md) ||
|| [onBookingUpdate](./on-booking-update.md) | When a booking is updated manually or via the method [booking.v1.booking.update](../booking-v1-booking-update.md) ||
|| [onBookingDelete](./on-booking-delete.md) | When a booking is deleted manually or via the method [booking.v1.booking.delete](../booking-v1-booking-delete.md), and also when a booking is moved to the waitlist via the method [booking.v1.waitlist.createfrombooking](../../waitlist/booking-v1-waitlist-createfrombooking.md) ||
|#