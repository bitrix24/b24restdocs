# Universal CRM Activities: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Universal activities are a type of activity with extended settings. In the universal activity card, you can synchronize the activity with the calendar, choose a meeting location with the client, add colleagues, select a client from the CRM object, categorize activity by color, and choose a meeting room. Some extended settings are available to the employee on the Bitrix24 side.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Universal activity in CRM](https://helpdesk.bitrix24.com/open/21458972/)

## Link with a CRM Entity

A universal activity is always created in the timeline of a specific CRM entity.

**CRM entity.** The link is defined by the `ownerTypeId` and `ownerId` parameters.

- `ownerTypeId` stores the type of the CRM object, for example `2` for a deal. You can find the type values in the [CRM Object Types](../../../data-types.md#object_type) reference.
- `ownerId` stores the identifier of the CRM entity. It is returned by the [crm.item.list](../../../universal/crm-item-list.md) and [crm.item.add](../../../universal/crm-item-add.md) methods.

**Another timeline activity.** A universal activity can be linked to an existing activity through the `parentActivityId` parameter of the [crm.activity.todo.add](./crm-activity-todo-add.md) method.

**User.** The employee specified in the `responsibleId` parameter is responsible for the activity. The responsible user can be changed with the [crm.activity.todo.updateResponsibleUser](./crm-activity-todo-update-responsible-user.md) method.

## How to Work with Universal Activities

1. Identify the CRM entity whose timeline will store the activity, and pass its type and identifier in `ownerTypeId` and `ownerId`.
2. Create the activity using the [crm.activity.todo.add](./crm-activity-todo-add.md) method. The `deadline` is required.
3. Retrieve the activity using the [crm.activity.get](../activity-base/crm-activity-get.md) method, or the list of activities of the entity using the [crm.activity.list](../activity-base/crm-activity-list.md) method with the filter `PROVIDER_ID` = `CRM_TODO`.
4. Update the entire activity using the [crm.activity.todo.update](./crm-activity-todo-update.md) method, or a single property using one of the `updateColor`, `updateDeadline`, `updateDescription`, or `updateResponsibleUser` methods.
5. Delete the activity using the [crm.activity.delete](../activity-base/crm-activity-delete.md) method.

## Which Update Method to Choose

#|
|| **What You Need** | **Method to Use** ||
|| Change several properties of the activity in a single call | [crm.activity.todo.update](./crm-activity-todo-update.md) ||
|| Change only the deadline | [crm.activity.todo.updateDeadline](./crm-activity-todo-update-deadline.md) ||
|| Change only the description | [crm.activity.todo.updateDescription](./crm-activity-todo-update-description.md) ||
|| Change only the color of the activity in the timeline | [crm.activity.todo.updateColor](./crm-activity-todo-update-color.md) ||
|| Change only the responsible user | [crm.activity.todo.updateResponsibleUser](./crm-activity-todo-update-responsible-user.md) ||
|#

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method. The `crm.activity.todo.*` methods are available to a user with permission to edit the CRM entity in whose timeline the activity is located

#|
|| **Method** | **Description** ||
|| [crm.activity.todo.add](./crm-activity-todo-add.md) | Adds a new universal activity to the timeline ||
|| [crm.activity.todo.update](./crm-activity-todo-update.md) | Updates the universal activity ||
|| [crm.activity.todo.updateColor](./crm-activity-todo-update-color.md) | Updates the color of the universal activity ||
|| [crm.activity.todo.updateDeadline](./crm-activity-todo-update-deadline.md) | Updates the deadline of the universal activity ||
|| [crm.activity.todo.updateDescription](./crm-activity-todo-update-description.md) | Updates the description of the universal activity ||
|| [crm.activity.todo.updateResponsibleUser](./crm-activity-todo-update-responsible-user.md) | Updates the responsible user for the universal activity ||
|| [crm.activity.get](../activity-base/crm-activity-get.md) | Retrieves information about the universal activity by its ID ||
|| [crm.activity.list](../activity-base/crm-activity-list.md) | Retrieves a list of universal activities for the CRM object with the filter `PROVIDER_ID` = `CRM_TODO` ||
|| [crm.activity.delete](../activity-base/crm-activity-delete.md) | Deletes the universal activity by its ID ||
|#

## Additional

- [{#T}](../../../data-types.md)
- [{#T}](../index.md)
