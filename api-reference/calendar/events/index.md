# Overview of Events When Working with Calendar Sections or Resources

Events allow applications to respond to changes almost in real-time: receiving notifications about the addition, modification, or deletion of a calendar section and resource.

Detailed information on working with events is described in the article [Concept and Benefits of Event Handling](../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to task events through:

-  [outgoing webhook](../../../local-integrations/local-webhooks.md)
-  [application](../../../settings/app-installation/index.md) and the method [event.bind](../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Handling Bitrix24 Events](../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered by** ||
|| [OnCalendarSectionAdd](./on-calendar-section-add.md) |
- When a calendar section is added manually or by the method [calendar.section.add](../calendar-section-add.md)
- When a resource is added manually or by the method [calendar.resource.add](../resource/calendar-resource-add.md) ||
|| [OnCalendarSectionUpdate](./on-calendar-section-update.md) | 
- When a calendar section is modified manually or by the method [calendar.section.update](../calendar-section-update.md)
- When a resource is modified manually or by the method [calendar.resource.update](../resource/calendar-resource-update.md) ||
|| [OnCalendarSectionDelete](./on-calendar-section-delete.md) | 
- When a calendar section is deleted manually or by the method [calendar.section.delete](../calendar-section-delete.md)
- When a resource is deleted manually or by the method [calendar.resource.delete](../resource/calendar-resource-delete.md) ||
|#