# Overview of CRM Custom Type Events

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Events allow applications to respond to changes in near real-time: receiving notifications about the addition, update, or deletion of CRM custom types.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to CRM custom type events through:

- [outgoing webhook](../../../../../local-integrations/local-webhooks.md)
- [application](../../../../../settings/app-installation/index.md) and the [event.bind](../../../../events/event-bind.md) method

An example of a handler code for the event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../../events/test-handler.md).

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can subscribe: `any user`

## Events

#| 
|| **Event** | **Triggered** ||
|| [onCrmTypeAdd](./on-crm-type-add.md) | When a CRM custom type is created manually or via the [crm.type.add](../../user-defined-object-types/crm-type-add.md) method ||
|| [onCrmTypeUpdate](./on-crm-type-update.md) | When a CRM custom type is updated manually or via the [crm.type.update](../../user-defined-object-types/crm-type-update.md) method ||
|| [onCrmTypeDelete](./on-crm-type-delete.md) | When a CRM custom type is deleted manually or via the [crm.type.delete](../../user-defined-object-types/crm-type-delete.md) method ||
|#