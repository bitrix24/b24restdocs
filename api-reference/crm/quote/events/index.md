# Overview of Events When Working with Estimates

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, or deletion of estimates.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to estimate events through:

- [outgoing webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [onCrmQuoteAdd](./on-crm-quote-add.md) | When an estimate is created manually or via the method [crm.quote.add](../crm-quote-add.md) ||
|| [onCrmQuoteUpdate](./on-crm-quote-update.md) | When an estimate is updated manually or via the method [crm.quote.update](../crm-quote-update.md) ||
|| [onCrmQuoteDelete](./on-crm-quote-delete.md) | When an estimate is deleted manually or via the method [crm.quote.delete](../crm-quote-delete.md) ||
|#