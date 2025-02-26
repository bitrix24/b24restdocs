# Overview of Methods

> Quick navigation: [all methods](#all-methods) 
> 
> User documentation: [Universal activity in CRM](https://helpdesk.bitrix24.com/open/21064046/)

Universal activities are a type of activity with extended settings. In the universal activity card, you can synchronize the activity with the calendar, choose a meeting location with the client, add colleagues, select a client from the CRM entity, categorize activity by color, and choose a meeting room. Some extended settings are available to the employee on the Bitrix24 side.

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user with the appropriate access permissions

## Overview of Methods {#all-methods}

#|
|| **Method** | **Description** ||
|| [crm.activity.todo.add](./crm-activity-todo-add.md) | Adds a new universal activity to the timeline ||
|| [crm.activity.todo.update](./crm-activity-todo-update.md) | Updates the universal activity ||
|| [crm.activity.todo.updateColor](./crm-activity-todo-update-color.md) | Updates the color of the universal activity ||
|| [crm.activity.todo.updateDeadline](./crm-activity-todo-update-deadline.md) | Updates the deadline of the universal activity ||
|| [crm.activity.todo.updateDescription](./crm-activity-todo-update-description.md) | Updates the description of the universal activity ||
|| [crm.activity.todo.updateResponsibleUser](./crm-activity-todo-update-responsible-user.md) | Updates the responsible user for the universal activity ||
|| [crm.activity.get](../activity-base/crm-activity-get.md) | Retrieves information about the universal activity by its ID ||
|| [crm.activity.list](../activity-base/crm-activity-list.md) | Retrieves a list of all universal activity for the CRM entity with the filter `PROVIDER_ID` = `"CRM_TODO"` ||
|| [crm.activity.delete](../activity-base/crm-activity-delete.md) | Deletes the universal activity by its ID ||
|#

## Additional

- [CRM Object Type](../../../data-types.md#object_type)