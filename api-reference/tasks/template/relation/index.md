# Task Template Relations: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A task template can be linked to other templates and to tasks. Child templates are linked to a parent template: when a task is created from the parent template, the child templates create subtasks. Related tasks of a template are the tasks on which the new task will depend; they are stored in the `dependsOn` field of the template.

The relation methods belong to REST 3.0 and are called at an address with `/rest/api/`. Call specifics and the response format are described in the [REST 3.0 overview](../../../rest-v3.md).

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [How to Work with Task Templates](https://helpdesk.bitrix24.com/open/20883624/)

## Getting Started

1. Retrieve the template identifier using the [tasks.template.list](../tasks-template-list-rest-v3.md) method or create a template using the [tasks.template.add](../tasks-template-add-rest-v3.md) method
2. To link child templates, call [tasks.template.relation.child.add](./tasks-template-relation-child-add-rest-v3.md). The current child templates are returned by the [tasks.template.relation.child.ids](./tasks-template-relation-child-ids-rest-v3.md) and [tasks.template.relation.child.list](./tasks-template-relation-child-list-rest-v3.md) methods; to unlink them, use the [tasks.template.relation.child.delete](./tasks-template-relation-child-delete-rest-v3.md) method
3. To link a template to tasks, call [tasks.template.relation.related.add](./tasks-template-relation-related-add-rest-v3.md). Related tasks are returned by the [tasks.template.relation.related.list](./tasks-template-relation-related-list-rest-v3.md) method; to unlink them, use the [tasks.template.relation.related.delete](./tasks-template-relation-related-delete-rest-v3.md) method
4. Create a task from the template using the [tasks.template.task.add](../tasks-template-task-add-rest-v3.md) method. To create subtasks from the child templates together with the task, pass `withSubTasks`

## Method Specifics

- Method names are written in lowercase: `tasks.template.relation.child.listbyids`, not `listByIds`. For a name in a different case, the method returns the `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION` error
- In a single call, you can pass up to 20 templates to the child template methods and up to 50 tasks to the related task methods
- A template can have only one parent template. A cycle of templates cannot be created: the method returns a relation conflict error
- The `list` and `ids` methods return only first-level child templates and only those the user can read

## Relationships with Other Objects

**Task templates.** Child templates are regular templates; their fields are described on the [Task Template Fields v 3.0](../fields-rest-v3.md) page. A template can be retrieved using the [tasks.template.get](../tasks-template-get-rest-v3.md) method.

**Tasks.** Related tasks of a template go to the `dependsOn` field of the task created from the template. Task fields are described on the [Task Fields in REST 3.0](../../fields-rest-v3.md) page, and the list of tasks is returned by the [tasks.task.list](../../tasks-task-list-rest-v3.md) method.

## Overview of Methods {#all-methods}

> Scope: [`tasks`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Child Templates

#|
|| **Method** | **Description** ||
|| [tasks.template.relation.child.add](./tasks-template-relation-child-add-rest-v3.md) | Links child templates to a parent template ||
|| [tasks.template.relation.child.delete](./tasks-template-relation-child-delete-rest-v3.md) | Unlinks child templates from a parent template ||
|| [tasks.template.relation.child.ids](./tasks-template-relation-child-ids-rest-v3.md) | Retrieves the IDs of child templates ||
|| [tasks.template.relation.child.list](./tasks-template-relation-child-list-rest-v3.md) | Retrieves a list of child templates with their fields ||
|| [tasks.template.relation.child.listbyids](./tasks-template-relation-child-list-by-ids-rest-v3.md) | Retrieves child templates for multiple templates ||
|#

### Related Tasks

#|
|| **Method** | **Description** ||
|| [tasks.template.relation.related.add](./tasks-template-relation-related-add-rest-v3.md) | Links tasks to a template ||
|| [tasks.template.relation.related.delete](./tasks-template-relation-related-delete-rest-v3.md) | Unlinks tasks from a template ||
|| [tasks.template.relation.related.list](./tasks-template-relation-related-list-rest-v3.md) | Retrieves a list of related tasks ||
|| [tasks.template.relation.related.listbyids](./tasks-template-relation-related-list-by-ids-rest-v3.md) | Retrieves related tasks by IDs ||
|#
