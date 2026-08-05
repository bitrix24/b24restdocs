# Overview of Events When Working with Waitlist Records

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, and deletion of records in the waitlist.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to waitlist entry events through:

- [outgoing webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

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
|| [onBookingWaitListItemDelete](./on-booking-waitlistitem-delete.md) | When a waitlist entry is deleted manually or by the method [booking.v1.waitlist.delete](../booking-v1-waitlist-delete.md) ||
|#
