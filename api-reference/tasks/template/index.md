# Task Templates: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Task templates help to describe a typical workflow in advance and use a unified set of settings for new tasks. A template can save the task title and description, participants, deadlines, project, recurrence, checklist, files, and CRM links.

Two API versions work with templates. Methods of the previous version are called at `/rest/`, and their fields are named in uppercase, for example, `RESPONSIBLE_ID`. REST 3.0 methods are called at `/rest/api/`, and their fields are named in camelCase, for example, `responsibleId`. Only REST 3.0 supports copying a template, files, recurrence history, creating a task from a template, child templates, and related tasks.

> Quick navigation: [all methods](#all-methods) | [REST 3.0 methods](#rest-v3)
>
> User documentation: [How to work with task templates](https://helpdesk.bitrix24.com/open/20883624/)

## Getting Started

1. Define the fields of the template on the [Task Template Fields](./fields.md) page or, for REST 3.0, on the [Task Template Fields v 3.0](./fields-rest-v3.md) page. In the previous version, the [tasks.template.fields](./tasks-template-fields.md) method returns the field descriptions.
2. Obtain the identifiers of users, groups, and CRM entities if references to these objects are needed in the template.
3. If the template is to create recurring tasks, prepare the recurrence parameters: `REPLICATE` and `REPLICATE_PARAMS` in the previous version, `replicate` and `replicateParams` in REST 3.0.
4. Create the template using the [tasks.template.add](./tasks-template-add.md) or [tasks.template.add](./tasks-template-add-rest-v3.md) v 3.0 method. To create a template based on an existing one, use [tasks.template.copy](./tasks-template-copy-rest-v3.md) v 3.0.
5. Check the saved values using the [tasks.template.get](./tasks-template-get.md) or [tasks.template.get](./tasks-template-get-rest-v3.md) v 3.0 method. The list of available templates is returned by [tasks.template.list](./tasks-template-list.md) or [tasks.template.list](./tasks-template-list-rest-v3.md) v 3.0.
6. Modify the template using the [tasks.template.update](./tasks-template-update.md) or [tasks.template.update](./tasks-template-update-rest-v3.md) v 3.0 method.
7. If the task consists of recurring steps, add a checklist to the template using the methods of the [Task Template Checklists](./checklist/index.md) section. Files are attached to a template by the [tasks.template.file.attach](./tasks-template-file-attach-rest-v3.md) v 3.0 method.
8. Enable recurrence using the [tasks.template.replication.setstate](./tasks-template-replication-set-state-rest-v3.md) v 3.0 method, or create a task from the template right away using the [tasks.template.task.add](./tasks-template-task-add-rest-v3.md) v 3.0 method.
9. Delete the template using the [tasks.template.delete](./tasks-template-delete.md) or [tasks.template.delete](./tasks-template-delete-rest-v3.md) v 3.0 method if it is no longer needed.

## Access Permissions

Permissions for working with task templates depend on the access settings in tasks:

- a user with the permission to create templates can create them
- a user with the corresponding permission for a template can retrieve, update, and delete it
- list methods return only the templates available to the user
- in REST 3.0, the access list of a template can be changed by an administrator, the template author, and a user with full access to the template
- the [tasks.template.access.get](./tasks-template-access-get-rest-v3.md) v 3.0 method returns the actions available to the current user for a template

{% note tip "User Documentation" %}

- [How to set access permissions in Bitrix24 tasks](https://helpdesk.bitrix24.com/open/20883624/)

{% endnote %}

## Method Specifics

### Previous Version

- The `TITLE` and `RESPONSIBLE_ID` fields are required when creating a template. If the method is called by a user without administrator rights, that user becomes the creator, and `CREATED_BY` is ignored
- The [tasks.template.list](./tasks-template-list.md) method returns up to 50 templates per request. Request the next page with the `start` parameter
- In `select` of the [tasks.template.list](./tasks-template-list.md) method, you can pass only the fields listed on the method page. The `*` value is not supported. The method returns custom `UF_*` fields and the checklist only if `select` is not passed
- The list of templates is returned in `task_templates`: an empty page as the `[]` array, a non-empty page as an object with template IDs as keys
- A successful response contains `result` and `time`; errors are returned in `error` and `error_description`. Error codes are described on the method pages

### REST 3.0

- When creating a template, `title`, `creatorId`, and an assignee in `responsibleId` or `responsibleCollection` are required. A user without administrator rights can specify only themselves as the creator
- The [tasks.template.list](./tasks-template-list-rest-v3.md) v 3.0 method returns only `id` without `select`. The page size is set by `pagination.limit`: 50 by default, 1000 at most. The filter accepts only conditions combined with `and`
- Boolean fields accept `true` and `false`; the values `Y` and `N` cause a validation error. Fields with a `null` value are not included in the response
- Method names are written in lowercase, for example, `tasks.template.replication.setstate`. For a name in a different case, the method returns the `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION` error
- A successful response contains the `result` object; errors are returned in the `error` object with the `code` and `message` fields

## Relationships with Other Objects

**Tasks.** The template stores the settings for the future task: title, description, deadlines, assignees, observers, checklist. A task is created from a template by the [tasks.template.task.add](./tasks-template-task-add-rest-v3.md) v 3.0 method, and [tasks.template.task.get](./tasks-template-task-get-rest-v3.md) v 3.0 shows what the task will look like without creating it. To work with tasks, use [tasks.task.add](../tasks-task-add.md), [tasks.task.get](../tasks-task-get.md), [tasks.task.update](../tasks-task-update.md), and [tasks.task.list](../tasks-task-list.md).

**Users.** In the template, you can specify the creator, assignees, participants, and observers: the `CREATED_BY`, `RESPONSIBLE_ID`, `RESPONSIBLES`, `ACCOMPLICES`, `AUDITORS` fields in the previous version and `creatorId`, `responsibleCollection`, `accomplices`, `auditors` in REST 3.0. User identifiers can be obtained using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods.

**Groups and Projects.** To link the template to a group or project, use the `GROUP_ID` field, in REST 3.0 — `groupId`. This allows you to create tasks in the desired workspace. The identifier of the group or project can be obtained using the [sonet_group.get](../../sonet-group/sonet-group-get.md) and [socialnetwork.api.workgroup.list](../../sonet-group/socialnetwork-api-workgroup-list.md) methods.

**CRM.** The template can pre-save links to CRM entities. In the `UF_CRM_TASK` field, in REST 3.0 — `crmItemIds`, specify the identifiers of CRM entities with a prefix, for example, `L_15` for a lead or `D_27` for a deal. Linked items are returned by the [tasks.template.crm.item.list](./tasks-template-crm-item-list-rest-v3.md) v 3.0 method. Identifiers of existing entities can be obtained using the [crm.item.list](../../crm/universal/crm-item-list.md) method. If the entity needs to be created first, use [crm.item.add](../../crm/universal/crm-item-add.md).

**Files.** In REST 3.0, Drive files are attached to a template by the [tasks.template.file.attach](./tasks-template-file-attach-rest-v3.md) v 3.0 method, and the list is returned by [tasks.template.file.list](./tasks-template-file-list-rest-v3.md) v 3.0. File identifiers can be obtained using the [disk.folder.getchildren](../../disk/folder/disk-folder-get-children.md) and [disk.folder.uploadfile](../../disk/folder/disk-folder-upload-file.md) methods.

**Checklists.** If the typical task consists of recurring steps, add them to the template. Checklist items are created, updated, and deleted using the methods of the [Task Template Checklists](./checklist/index.md) section.

**Child Templates and Related Tasks.** In REST 3.0, a template can be linked to another template as a child: when a task is created from the parent template, the child templates create subtasks. Related tasks of a template go to the `dependsOn` field of the future task. Links are changed using the methods of the [Task Template Relations](./relation/index.md) section.

## Overview of Methods {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#|
|| **Method** | **Description** ||
|| [tasks.template.add](./tasks-template-add.md) | Creates a task template and returns its ID ||
|| [tasks.template.update](./tasks-template-update.md) | Modifies the fields of an existing template ||
|| [tasks.template.get](./tasks-template-get.md) | Retrieves a template by ID. By default, with all fields, including custom ones ||
|| [tasks.template.list](./tasks-template-list.md) | Retrieves the templates available to the user with filtering, sorting, and pages of 50 records ||
|| [tasks.template.delete](./tasks-template-delete.md) | Deletes a task template ||
|| [tasks.template.fields](./tasks-template-fields.md) | Retrieves the description of template fields: title, type, whether the field is required, default value, and allowed values ||
|#

Checklist items of a template, their order, completion marks, and attachments are described in the [Task Template Checklists](./checklist/index.md) section.

## REST 3.0 Methods {#rest-v3}

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

REST 3.0 methods are called at an address with `/rest/api/`. Call specifics and the response format are described in the [REST 3.0 overview](../../rest-v3.md). Template fields in REST 3.0 are named in camelCase and described on the [Task Template Fields v 3.0](./fields-rest-v3.md) page. Method names are written in lowercase, for example, `tasks.template.replication.setstate`.

### Template

#|
|| **Method** | **Description** ||
|| [tasks.template.add](./tasks-template-add-rest-v3.md) | Creates a task template and returns it ||
|| [tasks.template.update](./tasks-template-update-rest-v3.md) | Updates template fields ||
|| [tasks.template.get](./tasks-template-get-rest-v3.md) | Retrieves a template by ID ||
|| [tasks.template.list](./tasks-template-list-rest-v3.md) | Retrieves a list of available templates ||
|| [tasks.template.copy](./tasks-template-copy-rest-v3.md) | Creates a copy of a template ||
|| [tasks.template.delete](./tasks-template-delete-rest-v3.md) | Deletes a template ||
|| [tasks.template.access.get](./tasks-template-access-get-rest-v3.md) | Retrieves the actions available to the user for a template ||
|| [tasks.template.crm.item.list](./tasks-template-crm-item-list-rest-v3.md) | Retrieves CRM items linked to a template ||
|#

### Files and History

#|
|| **Method** | **Description** ||
|| [tasks.template.file.list](./tasks-template-file-list-rest-v3.md) | Retrieves files attached to a template ||
|| [tasks.template.file.attach](./tasks-template-file-attach-rest-v3.md) | Attaches Drive files to a template ||
|| [tasks.template.file.detach](./tasks-template-file-detach-rest-v3.md) | Detaches files from a template ||
|| [tasks.template.history.list](./tasks-template-history-list-rest-v3.md) | Retrieves the recurrence history of a template ||
|| [tasks.template.history.count](./tasks-template-history-count-rest-v3.md) | Retrieves the number of history records of a template ||
|#

### Recurrence and Tasks from a Template

#|
|| **Method** | **Description** ||
|| [tasks.template.replication.setstate](./tasks-template-replication-set-state-rest-v3.md) | Enables or disables template recurrence ||
|| [tasks.template.state.set](./tasks-template-state-set-rest-v3.md) | Retains default values for the new template form ||
|| [tasks.template.task.get](./tasks-template-task-get-rest-v3.md) | Retrieves a task draft based on a template without creating the task ||
|| [tasks.template.task.add](./tasks-template-task-add-rest-v3.md) | Creates a task from a template ||
|#

### Checklist

#|
|| **Method** | **Description** ||
|| [tasks.template.checklist.get](./checklist/tasks-template-checklist-get-rest-v3.md) | Retrieves all checklist items of a template ||
|| [tasks.template.checklist.save](./checklist/tasks-template-checklist-save-rest-v3.md) | Retains the entire template checklist ||
|| [tasks.template.checklist.complete](./checklist/tasks-template-checklist-complete-rest-v3.md) | Marks all items as completed ||
|| [tasks.template.checklist.renew](./checklist/tasks-template-checklist-renew-rest-v3.md) | Returns all items to an incomplete state ||
|| [tasks.template.checklist.item.add](./checklist/tasks-template-checklist-item-add-rest-v3.md) | Adds an item ||
|| [tasks.template.checklist.item.update](./checklist/tasks-template-checklist-item-update-rest-v3.md) | Updates an item ||
|| [tasks.template.checklist.item.delete](./checklist/tasks-template-checklist-item-delete-rest-v3.md) | Deletes an item ||
|| [tasks.template.checklist.item.move](./checklist/tasks-template-checklist-item-move-rest-v3.md) | Moves an item ||
|| [tasks.template.checklist.item.complete](./checklist/tasks-template-checklist-item-complete-rest-v3.md) | Marks an item as completed ||
|| [tasks.template.checklist.item.renew](./checklist/tasks-template-checklist-item-renew-rest-v3.md) | Returns an item to an incomplete state ||
|| [tasks.template.checklist.item.collapse](./checklist/tasks-template-checklist-item-collapse-rest-v3.md) | Collapses an item ||
|| [tasks.template.checklist.item.expand](./checklist/tasks-template-checklist-item-expand-rest-v3.md) | Expands an item ||
|| [tasks.template.checklist.item.attachment.attach](./checklist/tasks-template-checklist-item-attachment-attach-rest-v3.md) | Attaches Drive files to an item ||
|| [tasks.template.checklist.item.attachment.upload](./checklist/tasks-template-checklist-item-attachment-upload-rest-v3.md) | Uploads a file to an item ||
|| [tasks.template.checklist.item.attachment.detach](./checklist/tasks-template-checklist-item-attachment-detach-rest-v3.md) | Detaches files from an item ||
|#

### Relations

For more information about relations, see the [Task Template Relations](./relation/index.md) section.

#|
|| **Method** | **Description** ||
|| [tasks.template.relation.child.add](./relation/tasks-template-relation-child-add-rest-v3.md) | Links child templates ||
|| [tasks.template.relation.child.delete](./relation/tasks-template-relation-child-delete-rest-v3.md) | Unlinks child templates ||
|| [tasks.template.relation.child.ids](./relation/tasks-template-relation-child-ids-rest-v3.md) | Retrieves the IDs of child templates ||
|| [tasks.template.relation.child.list](./relation/tasks-template-relation-child-list-rest-v3.md) | Retrieves a list of child templates ||
|| [tasks.template.relation.child.listbyids](./relation/tasks-template-relation-child-list-by-ids-rest-v3.md) | Retrieves child templates for multiple templates ||
|| [tasks.template.relation.related.add](./relation/tasks-template-relation-related-add-rest-v3.md) | Links tasks to a template ||
|| [tasks.template.relation.related.delete](./relation/tasks-template-relation-related-delete-rest-v3.md) | Unlinks tasks from a template ||
|| [tasks.template.relation.related.list](./relation/tasks-template-relation-related-list-rest-v3.md) | Retrieves a list of related tasks ||
|| [tasks.template.relation.related.listbyids](./relation/tasks-template-relation-related-list-by-ids-rest-v3.md) | Retrieves related tasks by IDs ||
|#
