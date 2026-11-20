# Task Templates: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

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
5. Retrieve the template using the [tasks.template.get](./tasks-template-get.md) method if you need to check the saved values. The [tasks.template.list](./tasks-template-list.md) method returns the list of available templates.
6. Modify the template using the [tasks.template.update](./tasks-template-update.md) method if you need to adjust the parameters.
7. If the task consists of recurring steps, add a checklist to the template using the methods of the [Task Template Checklists](./checklist/index.md) section.
8. Delete the template using the [tasks.template.delete](./tasks-template-delete.md) method if it is no longer needed.

## Access Permissions

Permissions for working with task templates depend on the access settings in tasks:

- a user with the permission to create templates can create them
- a user with the corresponding permission for a template can retrieve, update, and delete it
- the [tasks.template.list](./tasks-template-list.md) method returns only the templates available to the user

{% note tip "User Documentation" %}

- [How to set access permissions in Bitrix24 tasks](https://helpdesk.bitrix24.com/open/20883624/)

{% endnote %}

## Method Specifics

- The `TITLE` and `RESPONSIBLE_ID` fields are required when creating a template. If the method is called by a user without administrator rights, that user becomes the creator, and `CREATED_BY` is ignored
- The [tasks.template.list](./tasks-template-list.md) method returns up to 50 templates per request. Request the next page with the `start` parameter
- In `select` of the [tasks.template.list](./tasks-template-list.md) method, you can pass only the fields listed on the method page. The `*` value is not supported. The method returns custom `UF_*` fields and the checklist only if `select` is not passed
- The list of templates is returned in `task_templates`: an empty page as the `[]` array, a non-empty page as an object with template IDs as keys
- A successful response contains `result` and `time`; errors are returned in `error` and `error_description`. Error codes are described on the method pages

## Relationships with Other Objects

**Tasks.** The template stores the settings for the future task: title, description, deadlines, assignees, observers, checklist, and other parameters. To work with tasks, use [tasks.task.add](../tasks-task-add.md), [tasks.task.get](../tasks-task-get.md), [tasks.task.update](../tasks-task-update.md), and [tasks.task.list](../tasks-task-list.md).

**Users.** In the template, you can specify the Creator, main Participant, other Participants, and observers through the fields `CREATED_BY`, `RESPONSIBLE_ID`, `RESPONSIBLES`, `ACCOMPLICES`, `AUDITORS`. User identifiers can be obtained using the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods.

**Groups and Projects.** To link the template to a group or project, use the `GROUP_ID` field. This allows you to create tasks in the desired workspace. The identifier of the group or project can be obtained using the [sonet_group.get](../../sonet-group/sonet-group-get.md) and [socialnetwork.api.workgroup.list](../../sonet-group/socialnetwork-api-workgroup-list.md) methods.

**CRM.** The template can pre-save links to CRM entities. In the `UF_CRM_TASK` field, specify the identifiers of CRM entities with a prefix, for example, `L_15` for a lead or `D_27` for a deal. Identifiers of existing entities can be obtained using the [crm.item.list](../../crm/universal/crm-item-list.md) method. If the entity needs to be created first, use [crm.item.add](../../crm/universal/crm-item-add.md).

**Checklists.** If the typical task consists of recurring steps, add them to the template. Checklist items are created, updated, and deleted using the methods of the [Task Template Checklists](./checklist/index.md) section.

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
