# Overview of Events When Working with Contacts

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, or deletion of contacts.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to contact events through:

- [outgoing webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [onCrmContactAdd](./on-crm-contact-add.md) | When a contact is created manually or via the [crm.contact.add](../crm-contact-add.md) method ||
|| [onCrmContactUpdate](./on-crm-contact-update.md) | When a contact is updated manually or via the [crm.contact.update](../crm-contact-update.md) method ||
|| [onCrmContactDelete](./on-crm-contact-delete.md) | When a contact is deleted manually or via the [crm.contact.delete](../crm-contact-delete.md) method ||
|#