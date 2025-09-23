# Overview of Events When Working with Custom Contact Fields

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, or deletion of custom contact fields.

Detailed information on working with events is described in the article [Concept and Benefits of Event Handling](../../../../events/index.md).

> Quick navigation: [all events](#all-events) 

## How to Receive Events

You can subscribe to events for custom contact fields through:

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
|| **Event** | **Triggered** ||
|| [onCrmContactUserFieldAdd](./on-crm-contact-user-field-add.md) | When a custom field is added manually or via the method [crm.contact.userfield.add](../crm-contact-userfield-add.md) ||
|| [onCrmContactUserFieldUpdate](./on-crm-contact-user-field-update.md) | When a custom field is changed manually or via the method [crm.contact.userfield.update](../crm-contact-userfield-update.md) ||
|| [onCrmContactUserFieldDelete](./on-crm-contact-user-field-delete.md) | When a custom field is deleted manually or via the method [crm.contact.userfield.delete](../crm-contact-userfield-delete.md) ||
|| [onCrmContactUserFieldSetEnumValues](./on-crm-contact-user-field-set-enum-values.md) | When the set of values for a list-type custom field is changed manually or via the method [crm.contact.userfield.update](../crm-contact-userfield-update.md) ||
|#