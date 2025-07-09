# Overview of Events When Working with Resources

Events allow applications to respond to changes almost in real-time: receiving notifications about the creation, updating, and deletion of resources.

Detailed information on working with events is described in the article [Concept and Benefits of Event Handling](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to events in the section through:

- [outgoing webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Handling Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered By** ||
|| [onBookingResourceAdd](./on-booking-resource-add.md) | When a resource is created manually or by the method [booking.v1.resource.add](../booking-v1-resource-add.md) ||
|| [onBookingResourceUpdate](./on-booking-resource-update.md) | When a resource is updated manually or by the method [booking.v1.resource.update](../booking-v1-resource-update.md) ||
|| [onBookingResourceDelete](./on-booking-resource-delete.md) | When a resource is deleted manually or by the method [booking.v1.resource.delete](../booking-v1-resource-delete.md) ||
|#