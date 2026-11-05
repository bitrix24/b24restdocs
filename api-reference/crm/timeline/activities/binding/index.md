# Binding Deals with CRM Entities: Overview of Methods

An activity is shown in the timeline of the CRM item it is linked to. The `crm.activity.binding.*` methods add, move, and delete bindings of an existing activity and return a list of them.

For example, to move a customer's e-mail from the lead's timeline to the company's detail form, bind the activity to the company and then delete the binding to the lead.

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Timeline in the CRM Card](https://helpdesk.bitrix24.com/open/25816403/)

## Getting Started

1. Retrieve the `activityId` of the activity with the [crm.activity.list](../activity-base/crm-activity-list.md) method.
2. Determine the `entityTypeId` and `entityId` of the CRM entity.
3. Add the binding of the deal to the CRM entity using the [crm.activity.binding.add](./crm-activity-binding-add.md) method.
4. Check the activity bindings with the [crm.activity.binding.list](./crm-activity-binding-list.md) method.
5. If needed, move the binding to another item of the same type with the [crm.activity.binding.move](./crm-activity-binding-move.md) method.
6. Delete the unnecessary binding with the [crm.activity.binding.delete](./crm-activity-binding-delete.md) method.

## Operational Features

**Multiple Detail Forms.** One activity can be linked to multiple CRM items—it will be displayed in the timeline of each linked detail form.

**Binding Limit.** By default, an activity can have no more than 100 bindings. If you add a binding beyond the limit, Bitrix24 deletes the binding to the item with the lowest ID without returning an error. The [crm.activity.binding.list](./crm-activity-binding-list.md) method shows only bindings to items the user can read.

**Moving a Binding.** The [crm.activity.binding.move](./crm-activity-binding-move.md) method moves a binding only to an item of the same type: from one deal to another, but not from a deal to a contact.

**Deleting a Binding.** You cannot delete the only binding of an activity. To move such an activity to an item of a different type, first add a binding to the new item.

## Connection with Other Objects

**CRM Activities.** All methods in this section accept the activity identifier `activityId`. Take `activityId` from the response of the [crm.activity.todo.add](../todo/crm-activity-todo-add.md) method or retrieve it from the list using the [crm.activity.list](../activity-base/crm-activity-list.md) method.

**CRM Items.** A CRM item is specified by two parameters: the object type `entityTypeId` and the item identifier `entityId`. Where to get them:

- `entityTypeId` of a lead, deal, contact, or company — the [crm.enum.ownertype](../../../auxiliary/enum/crm-enum-owner-type.md) method
- `entityTypeId` of an SPA — the [crm.type.add](../../../universal/user-defined-object-types/crm-type-add.md) method when creating the type, or the [crm.type.list](../../../universal/user-defined-object-types/crm-type-list.md) method
- `entityId` — the [crm.item.list](../../../universal/crm-item-list.md) method

**Universal Activities.** A universal activity is an advanced type of activity that includes calendar synchronization, selection of a meeting place, and a meeting room. Its identifier from the [crm.activity.todo.add](../todo/crm-activity-todo-add.md) response is passed to the methods of this section as `activityId`.

**REST 3.0.** In REST 3.0, the same operations are performed by the [crm.activity.binding.bind](../../../rest-v3/activities/crm-activity-binding-bind-rest-v3.md), [unbind](../../../rest-v3/activities/crm-activity-binding-unbind-rest-v3.md), [move](../../../rest-v3/activities/crm-activity-binding-move-rest-v3.md), and [list](../../../rest-v3/activities/crm-activity-binding-list-rest-v3.md) methods.

{% note tip "Typical use-cases and scenarios" %}

- [How to move an activity between items of the same type](../../../../../tutorials/crm/how-to-edit-crm-objects/how-to-move-activity.md)
- [How to Move a Deal From One Object Type to Another](../../../../../tutorials/crm/how-to-edit-crm-objects/how-to-move-activity-between-objects.md)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [crm.activity.binding.add](./crm-activity-binding-add.md) | Adds a binding of the deal to a CRM entity ||
|| [crm.activity.binding.move](./crm-activity-binding-move.md) | Moves the binding of the deal to another CRM entity of the same type ||
|| [crm.activity.binding.list](./crm-activity-binding-list.md) | Returns a list of the deal's bindings ||
|| [crm.activity.binding.delete](./crm-activity-binding-delete.md) | Deletes the binding of the deal from the CRM entity ||
|#
