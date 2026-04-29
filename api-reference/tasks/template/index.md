# Task Templates: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Task templates help to describe a typical workflow in advance and use a unified set of settings for new tasks. A template can save the task title and description, participants, deadlines, project, recurrence, checklist, and other fields.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [How to work with task templates](https://helpdesk.bitrix24.com/open/20883624/)

## Getting Started

1. Define the fields of the template on the [Task Template Fields](./fields.md) page or obtain their description using the [tasks.template.fields](./tasks-template-fields.md) method.
2. If the template is to create recurring tasks, configure the `REPLICATE` and `REPLICATE_PARAMS` fields.
3. Obtain the identifiers of users, groups, and CRM entities if references to these objects are needed in the template.
4. Create the template using the [tasks.template.add](./tasks-template-add.md) method.
5. Retrieve the template using the [tasks.template.get](./tasks-template-get.md) method if you need to check the saved values.
6. Modify the template using the [tasks.template.update](./tasks-template-update.md) method if you need to adjust the parameters.
7. Add checklist items using the methods from the [Task Template Checklists](./checklist/index.md) section if the task consists of recurring steps.
8. Delete the template using the [tasks.template.delete](./tasks-template-delete.md) method if it is no longer needed.

## Access Permissions

Permissions for working with task templates depend on the access settings in tasks.

{% note tip "User Documentation" %}

- [How to set access permissions in Bitrix24 tasks](https://helpdesk.bitrix24.com/open/20883624/)

{% endnote %}

## Relationships with Other Objects

**Tasks.** The template stores the settings for the future task: title, description, deadlines, responsible persons, observers, checklist, and other parameters. Use the methods from the [Tasks](../index.md) section to work with tasks.

**Users.** In the template, you can specify the Creator, main Participant, other Participants, and observers through the fields `CREATED_BY`, `RESPONSIBLE_ID`, `RESPONSIBLES`, `ACCOMPLICES`, `AUDITORS`. User identifiers can be obtained using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods.

**Groups and Projects.** To link the template to a group or project, use the `GROUP_ID` field. This allows you to create tasks in the desired workspace. The identifier of the group or project can be obtained using the [sonet_group.get](../../sonet-group/sonet-group-get.md) and [socialnetwork.api.workgroup.list](../../sonet-group/socialnetwork-api-workgroup-list.md) methods.

**CRM.** The template can pre-save links to CRM entities. In the `UF_CRM_TASK` field, specify the identifiers of CRM entities with a prefix, for example, `L_15` for a lead or `D_27` for a deal. Identifiers of existing entities can be obtained using the [crm.item.list](../../crm/universal/crm-item-list.md) method. If the entity needs to be created first, use [crm.item.add](../../crm/universal/crm-item-add.md).

**Checklists.** If the typical task consists of recurring steps, add them to the template using the methods from the [Task Template Checklists](./checklist/index.md) subsection.

## Overview of Methods {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Main Methods

#|
|| **Method** | **Description** ||
|| [tasks.template.add](./tasks-template-add.md) | Adds a task template ||
|| [tasks.template.update](./tasks-template-update.md) | Modifies a task template ||
|| [tasks.template.get](./tasks-template-get.md) | Retrieves a task template by `id` ||
|| [tasks.template.delete](./tasks-template-delete.md) | Deletes a task template ||
|| [tasks.template.fields](./tasks-template-fields.md) | Retrieves the description of task template fields ||
|#

### Task Template Checklists

#|
|| **Method** | **Description** ||
|| [tasks.template.checklist.add](./checklist/tasks-template-checklist-add.md) | Adds a checklist item ||
|| [tasks.template.checklist.update](./checklist/tasks-template-checklist-update.md) | Updates a checklist item ||
|| [tasks.template.checklist.get](./checklist/tasks-template-checklist-get.md) | Retrieves a checklist item by `id` ||
|| [tasks.template.checklist.list](./checklist/tasks-template-checklist-list.md) | Retrieves a list of checklist items ||
|| [tasks.template.checklist.delete](./checklist/tasks-template-checklist-delete.md) | Deletes a checklist item ||
|| [tasks.template.checklist.moveAfter](./checklist/tasks-template-checklist-move-after.md) | Moves an item after the specified one ||
|| [tasks.template.checklist.moveBefore](./checklist/tasks-template-checklist-move-before.md) | Moves an item before the specified one ||
|| [tasks.template.checklist.complete](./checklist/tasks-template-checklist-complete.md) | Marks an item as completed ||
|| [tasks.template.checklist.renew](./checklist/tasks-template-checklist-renew.md) | Returns an item to an incomplete state ||
|| [tasks.template.checklist.addAttachmentByContent](./checklist/tasks-template-checklist-add-attachment-by-content.md) | Adds an attachment from content ||
|| [tasks.template.checklist.addAttachmentsFromDisk](./checklist/tasks-template-checklist-add-attachments-from-disk.md) | Adds attachments from Drive ||
|| [tasks.template.checklist.removeAttachments](./checklist/tasks-template-checklist-remove-attachments.md) | Removes attachments from a checklist item ||
|#