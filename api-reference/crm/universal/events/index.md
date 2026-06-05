# Overview of Events When Working with Smart Process Elements

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Events allow applications to respond to changes in near real-time: receiving notifications about the creation, updating, or deletion of smart process elements.

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to events of smart process elements through:

- [outgoing webhook](../../../../local-integrations/local-webhooks.md)
- [application](../../../../settings/app-installation/index.md) and the method [event.bind](../../../events/event-bind.md)

An example of a handler code for an event is described in the article [How to Test Your Handler for Processing Bitrix24 Events](../../../events/test-handler.md).

## How to Choose Which Events to Receive

There are two ways to receive smart process events: subscribe to basic events for all smart processes or to the event of a specific smart process.

**Basic Event for All Smart Processes.** If the handler needs to receive events for elements of all smart processes, subscribe to the events:

- [onCrmDynamicItemAdd](./on-crm-dynamic-item-add.md) — creation of a smart process element
- [onCrmDynamicItemUpdate](./on-crm-dynamic-item-update.md) — updating of a smart process element
- [onCrmDynamicItemDelete](./on-crm-dynamic-item-delete.md) — deletion of a smart process element

The basic event triggers for elements of different smart processes. The handler receives the `data.FIELDS` object:

- `ID` — identifier of the smart process element
- `ENTITY_TYPE_ID` — identifier of the smart process to which the element belongs

To process events of the desired smart process, check the value of `data.FIELDS.ENTITY_TYPE_ID`. The `entityTypeId` of the smart process can be obtained using the method [crm.type.list](../user-defined-object-types/crm-type-list.md).

**Event for a Single Smart Process.** If the handler needs to receive events for only a specific smart process, use the event with the suffix `_{entityTypeId}`. This suffix indicates which smart process the event relates to.

For example, for a smart process with `entityTypeId = 147`, the event for creating an element will be called `ONCRMDYNAMICITEMADD_147`.

Before subscribing, check if such an event is available:

- for the application — using the method [events](../../../events/events.md)
- for the outgoing webhook — in the [outgoing webhook builder](../../../../local-integrations/local-webhooks.md)

If the event with the suffix is not available, subscribe to the basic event [onCrmDynamicItemAdd](./on-crm-dynamic-item-add.md), [onCrmDynamicItemUpdate](./on-crm-dynamic-item-update.md), or [onCrmDynamicItemDelete](./on-crm-dynamic-item-delete.md). In the handler, check the `data.FIELDS.ENTITY_TYPE_ID` field: it contains the `entityTypeId` of the smart process to which the element belongs.

## Server Availability for Sending and Receiving Events

{% include notitle [Server Availability for Sending and Receiving Events](../../../../_includes/events-index.md) %}

## Overview of Events {#all-events}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

#| 
|| **Event** | **Triggered** ||
|| [onCrmDynamicItemAdd](./on-crm-dynamic-item-add.md) | When a smart process element is created manually or via the method [crm.item.add](../crm-item-add.md) ||
|| [onCrmDynamicItemUpdate](./on-crm-dynamic-item-update.md) | When a smart process element is updated manually or via the method [crm.item.update](../crm-item-update.md) ||
|| [onCrmDynamicItemDelete](./on-crm-dynamic-item-delete.md) | When a smart process element is deleted manually or via the method [crm.item.delete](../crm-item-delete.md) ||
|#