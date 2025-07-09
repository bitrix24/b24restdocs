# Overview of Events When Working with Resource Types

Events allow applications to respond to changes in near real-time: receiving notifications about the creation, updating, and deletion of resource types.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to section events through:

- [outgoing webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../app-installation/index.md) and the method [event.bind](../../../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`booking`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered By** ||
|| [onBookingResourceTypeAdd](./on-booking-resource-type-add.md) | When a resource type is created manually or by the method [booking.v1.resourcetype.add](../booking-v1-resourcetype-add.md) ||
|| [onBookingResourceTypeUpdate](./on-booking-resource-type-update.md) | When a resource type is updated by the method [booking.v1.resourcetype.update](../booking-v1-resourcetype-update.md) ||
|| [onBookingResourceTypeDelete](./on-booking-resource-type-delete.md) | When a resource type is deleted by the method [booking.v1.resourcetype.delete](../booking-v1-resourcetype-delete.md) ||
|#