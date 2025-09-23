# Overview of Events When Working with Waitlist Records

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, and deletion of records in the waitlist.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to events in the section through:

- [outgoing webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [onBookingWaitListItemAdd](./on-booking-waitlistitem-add.md) | When a record is created in the waitlist manually or via the methods [booking.v1.waitlist.add](../booking-v1-waitlist-add.md), [booking.v1.waitlist.createfrombooking](../booking-v1-waitlist-createfrombooking.md) ||
|| [onCrmDealUpdate](./on-booking-waitlistitem-update.md) | When a record in the waitlist is modified manually or via the method [booking.v1.waitlist.update](../booking-v1-waitlist-update.md) ||
|| [onBookingWaitListItemUpdate](./on-booking-waitlistitem-delete.md) | When a record is deleted from the waitlist manually or via the method [booking.v1.waitlist.delete](../booking-v1-waitlist-delete.md) ||
|#