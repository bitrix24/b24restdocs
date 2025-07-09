# Overview of Events When Working with Bookings

Events allow applications to respond to changes in near real-time: receiving notifications about the creation, updating, and deletion of bookings.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to events in the section through:

- [outgoing webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Events in Bitrix24](../../../events/test-handler.md).

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
|| [onBookingDelete](./on-booking-delete.md) | When a booking is deleted manually or via the method [booking.v1.booking.delete](../booking-v1-booking-delete.md) ||
|#