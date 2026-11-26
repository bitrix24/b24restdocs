# Overview of Events When Working with Deals

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, deletion, and movement of deals.

The create, update, and delete events pass only the deal identifier in the `data.FIELDS.ID` field. The [onCrmDealMoveToCategory](./on-crm-deal-move-to-category.md) event passes the new pipeline `CATEGORY_ID` and the new stage `STAGE_ID` along with the identifier. Request other deal data in the handler using [crm.deal.get](../crm-deal-get.md). After deletion, the deal is no longer available, so retain the data you need in advance.

Which event arrives when the pipeline changes depends on how it is changed:

- moving a deal from the deal form or with the "Change pipeline" automation rule triggers only [onCrmDealMoveToCategory](./on-crm-deal-move-to-category.md), without [onCrmDealUpdate](./on-crm-deal-update.md)
- the [crm.item.update](../../universal/crm-item-update.md) method with a new `categoryId` triggers only [onCrmDealUpdate](./on-crm-deal-update.md)

A stage change within the same pipeline arrives as [onCrmDealUpdate](./on-crm-deal-update.md).

Custom field configurations and recurring deal templates are tracked by separate groups of events: [events for custom deal fields](../user-defined-fields/events/index.md) and [events for recurring deals](../recurring-deals/events/index.md).

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to the events [onCrmDealAdd](./on-crm-deal-add.md), [onCrmDealUpdate](./on-crm-deal-update.md), and [onCrmDealDelete](./on-crm-deal-delete.md) through:

- [outbound webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

You can subscribe to the event [onCrmDealMoveToCategory](./on-crm-deal-move-to-category.md) only through the [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md).

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user. Administrator permissions are required for offline events

#|
|| **Event** | **Triggered By** ||
|| [onCrmDealAdd](./on-crm-deal-add.md) | When a deal is created manually or via the method [crm.deal.add](../crm-deal-add.md) ||
|| [onCrmDealUpdate](./on-crm-deal-update.md) | When a deal is modified manually or via the methods [crm.deal.update](../crm-deal-update.md) and [crm.item.update](../../universal/crm-item-update.md) ||
|| [onCrmDealDelete](./on-crm-deal-delete.md) | When a deal is deleted manually or via the method [crm.deal.delete](../crm-deal-delete.md) ||
|| [onCrmDealMoveToCategory](./on-crm-deal-move-to-category.md) | When a deal is moved to another pipeline from the deal form or with the "Change pipeline" automation rule ||
|#
