# Overview of Events When Working with Waitlist Records

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, and deletion of records in the waitlist.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

Booking events are described in the article [{#T}](../../booking/events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to waitlist entry events through:

- [outbound webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../settings/app-installation/installation-finish.md)

## What the Handler Receives

All three waitlist events pass only the record identifier in the `data` object. The identifier is passed as a string:

```json
{
    "event": "ONBOOKINGWAITLISTITEMUPDATE",
    "event_handler_id": "8",
    "data": {
        "ID": "1"
    },
    "ts": "1751285644"
}
```

The event contains neither field values nor a list of changes. To find out what has changed, retrieve the record using the [booking.v1.waitlist.get](../booking-v1-waitlist-get.md) method and compare the data with your copy. After the `onBookingWaitListItemDelete` event, this method returns the `1040` error with the description `Wait list not found`.

**Moving between a booking and the waitlist.** The [booking.v1.booking.createfromwaitlist](../../booking/booking-v1-booking-createfromwaitlist.md) method creates a booking and deletes the original waitlist record: `onBookingAdd` arrives first, followed by `onBookingWaitListItemDelete`. The [booking.v1.waitlist.createfrombooking](../booking-v1-waitlist-createfrombooking.md) method works the other way: `onBookingWaitListItemAdd` arrives first, followed by `onBookingDelete`.

The request also contains the `auth` key. It holds the authorization parameters and account data; its structure is described on the event page, for example [onBookingWaitListItemAdd](./on-booking-waitlistitem-add.md#auth).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered By** ||
|| [onBookingWaitListItemAdd](./on-booking-waitlistitem-add.md) | When a waitlist entry is created manually or by the methods [booking.v1.waitlist.add](../booking-v1-waitlist-add.md), [booking.v1.waitlist.createfrombooking](../booking-v1-waitlist-createfrombooking.md) ||
|| [onBookingWaitListItemUpdate](./on-booking-waitlistitem-update.md) | When a waitlist entry is changed manually or by the method [booking.v1.waitlist.update](../booking-v1-waitlist-update.md) ||
|| [onBookingWaitListItemDelete](./on-booking-waitlistitem-delete.md) | When a waitlist entry is deleted manually or by the method [booking.v1.waitlist.delete](../booking-v1-waitlist-delete.md), and also when a booking is created from the record by the method [booking.v1.booking.createfromwaitlist](../../booking/booking-v1-booking-createfromwaitlist.md) ||
|#
