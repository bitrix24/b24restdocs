# Overview of Events When Working with Custom Deal Fields

Events allow applications to respond to changes in near real-time: receiving notifications about the creation, update, or deletion of custom deal fields.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to events for custom deal fields through:

- [outgoing webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../app-installation/index.md) and the method [event.bind](../../../../events/event-bind.md)

An example of a handler code for the event is described in the article [How to Test Your Handler for Bitrix24 Event Processing](../../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered By** ||
|| [onCrmDealUserFieldAdd](./on-crm-deal-user-field-add.md) | When a custom field is added manually or via the method [crm.deal.userfield.add](../crm-deal-userfield-add.md) ||
|| [onCrmDealUserFieldUpdate](./on-crm-deal-user-field-update.md) | When a custom field is changed manually or via the method [crm.deal.userfield.update](../crm-deal-userfield-update.md) ||
|| [onCrmDealUserFieldDelete](./on-crm-deal-user-field-delete.md) | When a custom field is deleted manually or via the method [crm.deal.userfield.delete](../crm-deal-userfield-delete.md) ||
|| [onCrmDealUserFieldSetEnumValues](./on-crm-deal-user-field-set-enum-values.md) | When the set of values for a list-type custom field is changed manually or via the method [crm.deal.userfield.update](../crm-deal-userfield-update.md) ||
|#