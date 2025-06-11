# Overview of Events When Working with Recurring Deals

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, update, or deletion of recurring deals, as well as when new deals are automatically created based on recurring deal templates.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to recurring deal events through:

- [outgoing webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../app-installation/index.md) and the method [event.bind](../../../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [onCrmDealRecurringAdd](./on-crm-deal-recurring-add.md) | When a new recurring deal is created manually or via the method [crm.deal.recurring.add](../crm-deal-recurring-add.md) ||
|| [onCrmDealRecurringUpdate](./on-crm-deal-recurring-update.md) | When a recurring deal is modified manually or via the method [crm.deal.recurring.update](../crm-deal-recurring-update.md) ||
|| [onCrmDealRecurringDelete](./on-crm-deal-recurring-delete.md) | When a recurring deal is deleted manually or via the method [crm.deal.recurring.delete](../crm-deal-recurring-delete.md) ||
|| [onCrmDealRecurringExpose](./on-crm-deal-recurring-expose.md) | When a new deal is automatically created based on a recurring deal template or via the method [crm.deal.recurring.expose](../crm-deal-recurring-expose.md) ||
|#