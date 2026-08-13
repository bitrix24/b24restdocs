# Overview of Events When Working with Recurring Deals

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, update, and deletion of a [recurring deal template](../index.md), as well as about the creation of a new deal based on the template.

A deal created from a template is a regular deal, so the event [onCrmDealAdd](../../events/on-crm-deal-add.md) is triggered along with the event [onCrmDealRecurringExpose](./on-crm-deal-recurring-expose.md). To avoid processing the same deal twice, compare the identifier from the `DEAL_ID` field.

The events are not triggered when the fields of the template deal are modified. Such changes are tracked by the event [onCrmDealUpdate](../../events/on-crm-deal-update.md).

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to recurring deal events through:

- [outgoing webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../../settings/app-installation/index.md) and the method [event.bind](../../../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [onCrmDealRecurringAdd](./on-crm-deal-recurring-add.md) | When a recurring deal template is created manually or via the method [crm.deal.recurring.add](../crm-deal-recurring-add.md) ||
|| [onCrmDealRecurringUpdate](./on-crm-deal-recurring-update.md) | When the template configurations are modified manually or via the method [crm.deal.recurring.update](../crm-deal-recurring-update.md) ||
|| [onCrmDealRecurringDelete](./on-crm-deal-recurring-delete.md) | When the template is deleted manually or via the method [crm.deal.recurring.delete](../crm-deal-recurring-delete.md) ||
|| [onCrmDealRecurringExpose](./on-crm-deal-recurring-expose.md) | When a deal is created from the template on schedule or via the method [crm.deal.recurring.expose](../crm-deal-recurring-expose.md) ||
|#