# CRM Activities in the Timeline: Overview of Methods

A CRM activity is a record in the timeline that captures interactions with a client. It is linked to a CRM entity and displayed in its detail form.

The methods in this section work with CRM activities: they create and update universal activities, return an activity by its identifier, retrieve a list based on a filter, delete an activity, and describe fields.

For example, if you need to remove a call from the timeline of a deal, you would retrieve the list of activities for that deal and delete the desired activity.

{% note warning "" %}

**DEPRECATED**

The development of the methods [crm.activity.add](./crm-activity-add.md) and [crm.activity.update](./crm-activity-update.md) has been halted. Use the methods [crm.activity.todo.add](../todo/crm-activity-todo-add.md) and [crm.activity.todo.update](../todo/crm-activity-todo-update.md) instead.

{% endnote %}

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Timeline in CRM item form](https://helpdesk.bitrix24.com/open/25816403/)

## Getting Started

1. Define the CRM entity: `ownerTypeId` and `ownerId`
2. Create a universal activity using the method [crm.activity.todo.add](../todo/crm-activity-todo-add.md)
3. Verify the created activity using the method [crm.activity.get](./crm-activity-get.md)
4. To modify the universal activity, use the method [crm.activity.todo.update](../todo/crm-activity-todo-update.md)
5. To find activities based on a filter, use the method [crm.activity.list](./crm-activity-list.md)
6. Delete the desired activity using the method [crm.activity.delete](./crm-activity-delete.md)
7. To check the structure of the activity fields, use the method [crm.activity.fields](./crm-activity-fields.md)
8. Verify the structure of the communication fields using the method [crm.activity.communication.fields](./crm-activity-communication-fields.md)

## Linking to Other Objects

**CRM Entities.** An activity is linked to a CRM entity through the parameters `ownerTypeId` and `ownerId`. The typical values of `ownerTypeId` for leads, deals, contacts, and companies are returned by the method [crm.enum.ownertype](../../../auxiliary/enum/crm-enum-owner-type.md). For smart processes, use the method [crm.type.list](../../../universal/user-defined-object-types/crm-type-list.md). The identifier `ownerId` can be obtained through the universal method [crm.item.list](../../../universal/crm-item-list.md).

**Activity Links.** One activity can be linked to multiple CRM entities. To manage these links—adding, transferring, and deleting—use the methods from the group [crm.activity.binding.*](../binding/index.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [crm.activity.todo.add](../todo/crm-activity-todo-add.md) | Adds a universal activity to the timeline ||
|| [crm.activity.todo.update](../todo/crm-activity-todo-update.md) | Updates a universal activity ||
|| [crm.activity.get](./crm-activity-get.md) | Returns an activity by its identifier ||
|| [crm.activity.list](./crm-activity-list.md) | Returns a list of activities based on a filter ||
|| [crm.activity.delete](./crm-activity-delete.md) | Deletes an activity ||
|| [crm.activity.fields](./crm-activity-fields.md) | Returns the description of the activity fields ||
|| [crm.activity.communication.fields](./crm-activity-communication-fields.md) | Returns the description of the communication fields of the activity ||
|#

### Deprecated Methods

#| 
|| **Method** | **Description** ||
|| [crm.activity.add](./crm-activity-add.md) | Creates a system activity ||
|| [crm.activity.update](./crm-activity-update.md) | Updates a system activity ||
|#