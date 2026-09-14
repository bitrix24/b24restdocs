# Calendar Event Events: Overview of Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes almost in real-time: receiving notifications about the addition, modification, or deletion of a calendar event.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)
>
> User documentation: [How to Create an Event in the Calendar](https://helpdesk.bitrix24.com/open/21307284/)

## How to Receive Events

You can subscribe to calendar events through:

- [outbound webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

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
