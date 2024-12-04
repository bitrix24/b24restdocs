# Custom Fields in Tasks: Overview of Methods

In a task, there is a set of system fields: Assignee, watchers, due date, tags, and so on. If the system fields are insufficient, you can create your own—custom fields. Custom fields allow you to store information in various data formats: string, number, date with time, and yes/no.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [Custom fields for tasks](https://helpdesk.bitrix24.com/open/4884399)

## Features

When creating a custom field using the [task.item.userfield.add](./task-item-user-field-add.md) method, the field name `FIELD_NAME` must include the prefix `UF_`. If the prefix is not specified, the system will automatically add it to the beginning of the name.  
You can retrieve a list of all custom fields for tasks using the [task.item.userfield.getlist](./task-item-user-field-get-list.md) method. The list will include three system fields for tasks that link to other objects:

- `UF_CRM_TASK` —  with CRM objects
- `UF_MAIL_MESSAGE` —  with the email message
- `UF_TASK_WEBDAV_FILES` — with Drive files

These are created based on custom fields, so they appear in the list. More about the relationships of tasks with other objects can be found in the article [Tasks: Overview of Methods](../index.md).

## Overview of Methods {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#| 
|| **Method** | **Description** ||
|| [task.item.userfield.add](./task-item-user-field-add.md) | Creates a custom field ||
|| [task.item.userfield.update](./task-item-user-field-update.md) | Updates a custom field ||
|| [task.item.userfield.get](./task-item-user-field-get.md) | Retrieves a field by identifier `id` ||
|| [task.item.userfield.getlist](./task-item-user-field-get-list.md) | Retrieves a list of fields ||
|| [task.item.userfield.delete](./task-item-user-field-delete.md) | Deletes a custom field ||
|| [task.item.userfield.gettypes](./task-item-user-field-get-types.md) | Retrieves all available data types ||
|| [task.item.userfield.getfields](./task-item-user-field-get-fields.md) | Retrieves all available fields of the custom field ||
|#