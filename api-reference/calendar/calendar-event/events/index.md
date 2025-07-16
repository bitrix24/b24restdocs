# Overview of Events When Working with Calendar Events

Events allow applications to respond to changes almost in real-time: receiving notifications about the addition, modification, or deletion of a calendar event.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to task events through:

-  [outgoing webhook](../../../../local-integrations/local-webhooks.md)
-  [application](../../../app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`calendar`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [OnCalendarEntryAdd](./on-calendar-entry-add.md) | When a calendar event is added manually or via the method [calendar.event.add](../calendar-event-add.md) ||
|| [OnCalendarEntryUpdate](./on-calendar-entry-update.md) | When a calendar event is updated manually or via the method [calendar.event.update](../calendar-event-update.md) ||
|| [OnCalendarEntryDelete](./on-calendar-entry-delete.md) | When a calendar event is deleted manually or via the method [calendar.event.delete](../calendar-event-delete.md) ||
|#