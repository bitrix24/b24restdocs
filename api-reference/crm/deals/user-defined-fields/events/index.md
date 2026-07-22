# Overview of Events When Working with Custom Deal Fields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Events allow applications to receive notifications regarding changes to custom deal field configurations: creating, updating, or deleting a field, as well as modifying the set of values for a list-type field.

They are not triggered when the value of a custom field is changed within a specific deal. To track a field value change in a deal, subscribe to the [onCrmDealUpdate](../../events/on-crm-deal-update.md) deal update event. The current field value can be retrieved using the [crm.deal.get](../../crm-deal-get.md) method.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../../events/index.md).

> Quick navigation: [All Events](#all-events)

## How to Receive Events

You can subscribe to events for custom deal fields through:

- [Outgoing webhook](../../../../../local-integrations/local-webhooks.md)
- [Application](../../../../../settings/app-installation/index.md) and the [event.bind](../../../../events/event-bind.md) method

An example of a handler for the event is described in the article [How to Test Your Handler for Processing Events in Bitrix24](../../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server availability](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: Any user

#|
|| **Event** | **Triggered** ||
|| [onCrmDealUserFieldAdd](./on-crm-deal-user-field-add.md) | When a custom field is added manually or via the method [crm.deal.userfield.add](../crm-deal-userfield-add.md) ||
|| [onCrmDealUserFieldUpdate](./on-crm-deal-user-field-update.md) | When a custom field is changed manually or via the method [crm.deal.userfield.update](../crm-deal-userfield-update.md) ||
|| [onCrmDealUserFieldDelete](./on-crm-deal-user-field-delete.md) | When a custom field is deleted manually or via the method [crm.deal.userfield.delete](../crm-deal-userfield-delete.md) ||
|| [onCrmDealUserFieldSetEnumValues](./on-crm-deal-user-field-set-enum-values.md) | When the set of values for a list-type custom field is changed manually or via the method [crm.deal.userfield.update](../crm-deal-userfield-update.md) ||
|#
