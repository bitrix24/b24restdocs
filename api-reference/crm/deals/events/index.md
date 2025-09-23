# Overview of Events When Working with Deals

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, deletion, and movement of deals.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to the events [onCrmDealAdd](./on-crm-deal-add.md), [onCrmDealUpdate](./on-crm-deal-update.md), and [onCrmDealDelete](./on-crm-deal-delete.md) through:

- [outgoing webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

You can subscribe to the event [onCrmDealMoveToCategory](./on-crm-deal-move-to-category.md) only through the [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md).

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered By** ||
|| [onCrmDealAdd](./on-crm-deal-add.md) | When a deal is created manually or via the method [crm.deal.add](../crm-deal-add.md) ||
|| [onCrmDealUpdate](./on-crm-deal-update.md) | When a deal is modified manually or via the method [crm.deal.update](../crm-deal-update.md) ||
|| [onCrmDealDelete](./on-crm-deal-delete.md) | When a deal is deleted manually or via the method [crm.deal.delete](../crm-deal-delete.md) ||
|| [onCrmDealMoveToCategory](./on-crm-deal-move-to-category.md) | When the deal's funnel is changed manually or via the method [crm.item.update](../../universal/crm-item-update.md) ||
|#