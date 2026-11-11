# Overview of Events When Working with Recurring Deals

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation and deletion of a [recurring deal template](../index.md), changes to its recurrence settings, and the creation of a new deal from the template.

Changes to the fields of the template deal, such as its title or amount, are tracked by the [onCrmDealUpdate](../../events/on-crm-deal-update.md) event.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to recurring deal events through:

- [outbound webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../../settings/app-installation/index.md) and the method [event.bind](../../../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../../events/test-handler.md).

{% note info "" %}

Events will not be sent to the application until the installation is complete. [Check the application installation](../../../../../settings/app-installation/installation-finish.md)

{% endnote %}

## What the Handler Receives

All events pass only identifiers in the `data.FIELDS` object.

#|
|| **Parameter** | **Meaning** | **Events** ||
|| `ID` | Identifier of the recurrence settings | All four events ||
|| `RECURRING_DEAL_ID` | Identifier of the template deal. Matches the `DEAL_ID` field in the [crm.deal.recurring.get](../crm-deal-recurring-get.md) response | `onCrmDealRecurringAdd`, `onCrmDealRecurringUpdate`, `onCrmDealRecurringExpose` ||
|| `DEAL_ID` | Identifier of the deal created from the template | `onCrmDealRecurringExpose` ||
|#

A full request example is given on the page of each event, for example [onCrmDealRecurringExpose](./on-crm-deal-recurring-expose.md).

The recurrence settings are returned by the [crm.deal.recurring.get](../crm-deal-recurring-get.md) method, and the fields of the template deal and the created deal by the [crm.deal.get](../../crm-deal-get.md) method.

After the `onCrmDealRecurringDelete` event, the settings no longer exist: the `crm.deal.recurring.get` method returns the `Recurring deal is not found` error.

## Order of Events {#order}

Actions with a template trigger events in the following order:

- creating a template: [onCrmDealAdd](../../events/on-crm-deal-add.md) with the `ID` of the template deal, then `onCrmDealRecurringAdd`
- creating a deal from the template: [onCrmDealAdd](../../events/on-crm-deal-add.md) with the `ID` of the new deal, `onCrmDealRecurringExpose`, then `onCrmDealRecurringUpdate`
- a scheduled deal could not be created: `onCrmDealRecurringUpdate` without `onCrmDealRecurringExpose`
- deleting the template deal: `onCrmDealRecurringDelete`, then [onCrmDealDelete](../../events/on-crm-deal-delete.md)

To avoid processing a new deal twice, match `DEAL_ID` from `onCrmDealRecurringExpose` against `ID` from `onCrmDealAdd`.

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [onCrmDealRecurringAdd](./on-crm-deal-recurring-add.md) | When a recurring deal template is created manually or via the method [crm.deal.recurring.add](../crm-deal-recurring-add.md) ||
|| [onCrmDealRecurringUpdate](./on-crm-deal-recurring-update.md) | When the recurrence settings are modified manually or via the method [crm.deal.recurring.update](../crm-deal-recurring-update.md), after a deal is created from the template, and when a scheduled deal could not be created. The order of calls is described in [Order of Events](#order) ||
|| [onCrmDealRecurringDelete](./on-crm-deal-recurring-delete.md) | When the template deal is deleted manually or via the method [crm.deal.delete](../../crm-deal-delete.md) or [crm.item.delete](../../../universal/crm-item-delete.md). The [crm.deal.recurring.delete](../crm-deal-recurring-delete.md) method deletes the settings and triggers the event only if the template deal no longer exists ||
|| [onCrmDealRecurringExpose](./on-crm-deal-recurring-expose.md) | When a deal is created from the template, on schedule or via the method [crm.deal.recurring.expose](../crm-deal-recurring-expose.md) ||
|#
