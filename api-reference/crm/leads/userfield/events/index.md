# Overview of Events When Working with Custom Lead Fields

Events allow applications to respond to changes in near real-time: receiving notifications about the creation, update, or deletion of custom lead fields.

Detailed information on working with events is described in the article [Concept and Benefits of Event Handling](../../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to events for custom lead fields through:

- [outgoing webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../../settings/app-installation/index.md) and the method [event.bind](../../../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Bitrix24 Event Processing](../../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered By** ||
|| [onCrmLeadUserFieldAdd](./on-crm-lead-user-field-add.md) | When a custom field is added manually or via the method [crm.lead.userfield.add](../crm-lead-userfield-add.md) ||
|| [onCrmLeadUserFieldUpdate](./on-crm-lead-user-field-update.md) | When a custom field is changed manually or via the method [crm.lead.userfield.update](../crm-lead-userfield-update.md) ||
|| [onCrmLeadUserFieldDelete](./on-crm-lead-user-field-delete.md) | When a custom field is deleted manually or via the method [crm.lead.userfield.delete](../crm-lead-userfield-delete.md) ||
|| [onCrmLeadUserFieldSetEnumValues](./on-crm-lead-user-field-set-enum-values.md) | When the set of values for a list-type custom field is changed manually or via the method [crm.lead.userfield.update](../crm-lead-userfield-update.md) ||
|#