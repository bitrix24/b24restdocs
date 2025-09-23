# Overview of Events When Working with Custom Fields in Estimates

Events allow applications to respond to changes almost in real-time: receiving notifications about the creation, updating, or deletion of custom fields in estimates.

Detailed work with events is described in the article [Concept and Benefits of Event Handling](../../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to events for custom fields in estimates through:

- [outgoing webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../../settings/app-installation/index.md) and the method [event.bind](../../../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Handling Bitrix24 Events](../../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [onCrmQuoteUserFieldAdd](./on-crm-quote-user-field-add.md) | When a custom field is added manually or via the method [crm.quote.userfield.add](../crm-quote-user-field-add.md) ||
|| [onCrmQuoteUserFieldUpdate](./on-crm-quote-user-field-update.md) | When a custom field is updated manually or via the method [crm.quote.userfield.update](../crm-quote-user-field-update.md) ||
|| [onCrmQuoteUserFieldDelete](./on-crm-quote-user-field-delete.md) | When a custom field is deleted manually or via the method [crm.quote.userfield.delete](../crm-quote-user-field-delete.md) ||
|| [onCrmQuoteUserFieldSetEnumValues](./on-crm-quote-user-field-set-enum-values.md) | When the set of values for a list-type custom field is changed manually or via the method [crm.quote.userfield.update](../crm-quote-user-field-update.md) ||
|#