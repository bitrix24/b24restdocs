# Custom Fields in Tasks: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In a task, there is a set of system fields: Assignee, watchers, due date, tags, and so on. If the system fields are insufficient, you can create your own—custom fields. Custom fields allow you to store information in various data formats: string, number, date with time, and yes/no.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Custom Fields for Tasks](https://helpdesk.bitrix24.com/open/4884399)

## How to Get Started

1. Retrieve the available custom field types using the [task.item.userfield.gettypes](./task-item-user-field-get-types.md) method.
2. Retrieve field descriptions for configuring a custom field using the [task.item.userfield.getfields](./task-item-user-field-get-fields.md) method.
3. Create a custom field using the [task.item.userfield.add](./task-item-user-field-add.md) method.
4. Check the created field using the [task.item.userfield.get](./task-item-user-field-get.md) method or retrieve a list of fields using the [task.item.userfield.getlist](./task-item-user-field-get-list.md) method.
5. Update field parameters using the [task.item.userfield.update](./task-item-user-field-update.md) method if you need to adjust its name, required status, or multiplicity.
6. Delete the field using the [task.item.userfield.delete](./task-item-user-field-delete.md) method if it is no longer needed.

## Custom Field Features

When creating a custom field using the [task.item.userfield.add](./task-item-user-field-add.md) method, the field name `FIELD_NAME` must include the prefix `UF_`. If the prefix is not specified, the system will automatically add it to the beginning of the name.
You can retrieve a list of all custom fields for tasks using the [task.item.userfield.getlist](./task-item-user-field-get-list.md) method. The list will include three system fields for tasks that link to other objects:

- `UF_CRM_TASK` — with CRM objects
- `UF_MAIL_MESSAGE` — with the email message
- `UF_TASK_WEBDAV_FILES` — with Drive files

These are created based on custom fields, so they appear in the list. More about the relationships of tasks with other objects can be found in the article [Tasks: Overview of Methods](../index.md).

## Relationship with Other Objects

**Task.** Custom fields are added to the task card and are available in task methods. Custom field values are passed in the task field array, for example `UF_CRM_TASK` or a custom field with the `UF_` prefix.

**CRM.** The system field `UF_CRM_TASK` links a task to CRM objects. To link an object, specify its identifier with a prefix, for example `C_3` for a contact or `D_27` for a deal. You can retrieve CRM item identifiers using the [crm.item.list](../../crm/universal/crm-item-list.md) method.

**Drive.** The system field `UF_TASK_WEBDAV_FILES` links a task to Drive files. To attach files to an already created task, use the [tasks.task.files.attach](../tasks-task-files-attach.md) method.

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
