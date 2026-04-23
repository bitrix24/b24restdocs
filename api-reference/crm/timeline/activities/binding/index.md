# Binding Deals with CRM Entities: Overview of Methods

The group of methods `crm.activity.binding.*` works with the relationships of an existing deal to CRM entities: it adds a binding, returns a list, moves a binding to another entity, or deletes it.

For example, you may need to move a client's e-mail from the lead's timeline to the company's detail form. To do this, you add a binding with the company—the deal will then appear in its timeline. After that, you can remove the binding with the lead.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Timeline in CRM item form](https://helpdesk.bitrix24.com/open/25816403/)

## Getting Started

1. Obtain the `activityId` of the deal.
2. Determine the `entityTypeId` and `entityId` of the CRM entity.
3. Add the binding of the deal to the CRM entity using the [crm.activity.binding.add](./crm-activity-binding-add.md) method.
4. To check the current bindings of the deal, use the [crm.activity.binding.list](./crm-activity-binding-list.md) method.
5. If you need to move the binding to another entity of the same type, use the [crm.activity.binding.move](./crm-activity-binding-move.md) method.
6. Delete the required binding using the [crm.activity.binding.delete](./crm-activity-binding-delete.md) method.

## Operational Features

**Multiple detail forms.** One deal can be linked to multiple CRM entities, but no more than 100—it will be displayed in the timeline of each linked detail form.

**Moving a binding.** The [crm.activity.binding.move](./crm-activity-binding-move.md) method changes the binding only between entities of the same type: from one deal to another, but not from a deal to a contact.

**Deleting a binding.** You cannot delete a binding if it is the only one for the deal.

## Binding with Other Objects

**CRM Deals.** All methods in this section accept the deal identifier `activityId`. Obtain `activityId` from the response of the [crm.activity.add](../activity-base/crm-activity-add.md) method or retrieve it from the list using the [crm.activity.list](../activity-base/crm-activity-list.md) method.

**CRM Entities.** To bind a deal to a CRM entity, pass `entityTypeId` and `entityId`. The typical values of `entityTypeId` for leads, deals, contacts, and companies are returned by the [crm.enum.ownertype](../../../auxiliary/enum/crm-enum-owner-type.md) method. For Smart Processes, `entityTypeId` is obtained when creating the type using the [crm.type.add](../../../universal/user-defined-object-types/crm-type-add.md) method or from the list using the [crm.type.list](../../../universal/user-defined-object-types/crm-type-list.md) method. The identifier of the required entity `entityId` can be obtained using the universal [crm.item.list](../../../universal/crm-item-list.md) method.

**Universal Deals.** A universal deal is an advanced type of deal that includes synchronization with a calendar, selection of a meeting place, and negotiation room. A universal deal is created using the [crm.activity.todo.add](../todo/crm-activity-todo-add.md) method. This method returns the identifier of the created deal, which is passed to the `crm.activity.binding.*` methods as `activityId`.

{% note tip "Typical use-cases and scenarios" %}

- [How to move a deal from one object type to another](../../../../../tutorials/crm/how-to-edit-crm-objects/how-to-move-activity.md)
- [How to move a deal between CRM objects](../../../../../tutorials/crm/how-to-edit-crm-objects/how-to-move-activity-between-objects.md)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can perform the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [crm.activity.binding.add](./crm-activity-binding-add.md) | Adds a binding of the deal to a CRM entity ||
|| [crm.activity.binding.move](./crm-activity-binding-move.md) | Moves the binding of the deal to another CRM entity of the same type ||
|| [crm.activity.binding.list](./crm-activity-binding-list.md) | Returns a list of the deal's bindings ||
|| [crm.activity.binding.delete](./crm-activity-binding-delete.md) | Deletes the binding of the deal from the CRM entity ||
|#