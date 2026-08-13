# Overview of Events When Working with Contacts

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Events allow applications to respond to changes in almost real-time: receiving notifications about the creation, updating, or deletion of [contacts](../index.md).

In all three events, the handler receives only the contact identifier in `data.FIELDS.ID`, without the field values. After a contact is created or updated, request its data with the method [crm.contact.get](../crm-contact-get.md) or [crm.item.get](../../universal/crm-item-get.md) with `entityTypeId = 3`. After deletion, the contact is no longer available, so retain the data you need on your side in advance.

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
|| [onCrmContactAdd](./on-crm-contact-add.md) | When a contact is created manually, via the [crm.contact.add](../crm-contact-add.md) method, or via the [crm.item.add](../../universal/crm-item-add.md) method with `entityTypeId = 3` ||
|| [onCrmContactUpdate](./on-crm-contact-update.md) | When a contact is updated manually, via the [crm.contact.update](../crm-contact-update.md) method, or via the [crm.item.update](../../universal/crm-item-update.md) method with `entityTypeId = 3` ||
|| [onCrmContactDelete](./on-crm-contact-delete.md) | When a contact is deleted manually, via the [crm.contact.delete](../crm-contact-delete.md) method, or via the [crm.item.delete](../../universal/crm-item-delete.md) method with `entityTypeId = 3` ||
|#