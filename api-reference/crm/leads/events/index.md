# Overview of Events When Working with Leads

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, update, or deletion of leads.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to lead events through:

- [outgoing webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered By** ||
|| [onCrmLeadAdd](./on-crm-lead-add.md) | When a lead is added manually or via the [crm.lead.add](../crm-lead-add.md) method ||
|| [onCrmLeadUpdate](./on-crm-lead-update.md) | When a lead is updated manually or via the [crm.lead.update](../crm-lead-update.md) method ||
|| [onCrmLeadDelete](./on-crm-lead-delete.md) | When a lead is deleted manually or via the [crm.lead.delete](../crm-lead-delete.md) method ||
|#