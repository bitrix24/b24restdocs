# Overview of Events When Working with Custom Fields in Estimates

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Events allow applications to respond to changes almost in real-time: receiving notifications about the creation, updating, or deletion of custom fields in estimates. The methods for working with these fields are collected in the section [Custom Fields for Estimates](../index.md).

Detailed work with events is described in the article [Concept and Benefits of Event Handling](../../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to events for custom fields in estimates through:

- [outgoing webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../../settings/app-installation/index.md) and the method [event.bind](../../../../events/event-bind.md)

An example of a handler for the event is described in the article [How to Test Your Handler for Handling Bitrix24 Events](../../../../events/test-handler.md).

## What the Handler Receives

Custom field events do not pass the entire field description — the handler receives the key by which the field can be requested with a method.

`data.FIELDS` contains three values:

- `ID` — the identifier of the custom field. The full description of the field is returned by the method [crm.quote.userfield.get](../crm-quote-user-field-get.md) using this identifier
- `ENTITY_ID` — the symbolic identifier of the object the field belongs to. For estimates, it is always `CRM_QUOTE`
- `FIELD_NAME` — the full name of the field with the prefix `UF_CRM_`. The value of the field is passed to the methods [crm.quote.add](../../crm-quote-add.md) and [crm.quote.update](../../crm-quote-update.md) under this name

All four events arrive with the same set of fields. Distinguish them by the event code in the `event` field, not by the composition of `data.FIELDS`. After a field is deleted, its description can no longer be retrieved by identifier.

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