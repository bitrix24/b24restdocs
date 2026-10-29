# Overview of Events When Working with Smart Process Elements

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Events allow applications to respond to changes in near real-time: receiving notifications about the creation, updating, or deletion of smart process elements.

The events of this section track elements. Changes to the smart processes themselves are tracked by the [CRM custom type events](./type/index.md), and changes to their custom fields by the [custom field settings events](../userfieldconfig/events/index.md).

Detailed information on working with events is described in the article [Concept and Benefits of Event Processing](../../../events/index.md).

> Quick navigation: [all events](#all-events)

## How to Receive Events

You can subscribe to events of smart process elements through:

- [outbound webhook](../../../../local-integrations/local-webhooks.md)
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

An example of the data the handler receives when a smart process element with `entityTypeId = 1220` is created. The `auth` parameter is omitted; the full example is on the [onCrmDynamicItemAdd](./on-crm-dynamic-item-add.md) event page:

```json
{
    "event": "ONCRMDYNAMICITEMADD",
    "event_handler_id": "4",
    "data": {
        "FIELDS": {
            "ID": "22",
            "ENTITY_TYPE_ID": "1220"
        }
    },
    "ts": "1723534033"
}
```

The values arrive as strings. For an event with the `_{entityTypeId}` suffix, `data.FIELDS` has the same content, and `event` contains the name with the suffix.

Retrieve all fields of the element using the [crm.item.get](../crm-item-get.md) method: pass `entityTypeId` and `id` from `data.FIELDS`. After the element is deleted, the method no longer returns its data.

To process events of the desired smart process, check the value of `data.FIELDS.ENTITY_TYPE_ID`. The `entityTypeId` of the smart process can be obtained using the method [crm.type.list](../user-defined-object-types/crm-type-list.md).

**Event for a Single Smart Process.** If the handler needs to receive events for only a specific smart process, use the event with the suffix `_{entityTypeId}`. This suffix indicates which smart process the event relates to.

For example, for a smart process with `entityTypeId = 147`, the event for creating an element will be called `ONCRMDYNAMICITEMADD_147`.

Before subscribing, check if such an event is available:

- for the application — using the method [events](../../../events/events.md)
- for the outbound webhook — in the [outbound webhook builder](../../../../local-integrations/local-webhooks.md)

If the event with the suffix is not available, subscribe to the basic events and check `data.FIELDS.ENTITY_TYPE_ID` in the handler.

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