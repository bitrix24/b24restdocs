# Workflow Tasks: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Workflows can create tasks for participants to gather additional information from them. For example, a task can be used to approve a document in accounting, authorize a vacation with a manager, or request a contract from a lawyer. Users can see their tasks in Bitrix24 and receive notifications.

There are four types of tasks:

- Document approval
- Document review
- Request for additional information
- Request for additional information with rejection

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Workflow Assignments](https://helpdesk.bitrix24.com/open/11466058/)

## How to Get Started

1. Retrieve the list of tasks using the [bizproc.task.list](./bizproc-task-list.md) method
2. Find the identifier of the required task `TASK_ID`
3. Complete the task using the [bizproc.task.complete](./bizproc-task-complete.md) method
4. If needed, delegate the task to another employee using the [bizproc.task.delegate](./bizproc-task-delegate.md) method

## Task Fields

Tasks that request additional information are completed using the [bizproc.task.complete](./bizproc-task-complete.md) method. Field values are passed in the `FIELDS` object in the format `{"field_1": "value_1", ... , "field_N": "value_N"}`:

- `field_N` — symbolic identifier of the task field
- `value_N` — value of the field

You can find out which fields must be filled in from the response of the [bizproc.task.list](./bizproc-task-list.md) method. The `PARAMETERS.Fields` field contains descriptions of all task fields.

## What to Consider

- You can complete only your own tasks
- To complete a task, you need the `TASK_ID` identifier, which can be obtained using the [bizproc.task.list](./bizproc-task-list.md) method
- Tasks that request additional information contain fields that the user must fill in
- To delegate a task, you need the task identifier `TASK_IDS`, the current assignee identifier `FROM_USER_ID`, and the new assignee identifier `TO_USER_ID`

## Relationship with Other Objects

**User.** A task is linked to an assignee. To delegate it, pass the current assignee identifier `FROM_USER_ID` and the new assignee identifier `TO_USER_ID` to the [bizproc.task.delegate](./bizproc-task-delegate.md) method. You can get the user identifier using the [user.get](../../user/user-get.md) method.

**Workflow.** A task is created while a workflow is running. You can retrieve the list of tasks available to the user and their parameters using the [bizproc.task.list](./bizproc-task-list.md) method.

## Overview of Methods {#all-methods}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [bizproc.task.list](./bizproc-task-list.md) | Retrieves a list of workflow tasks ||
|| [bizproc.task.complete](./bizproc-task-complete.md) | Completes a workflow task ||
|| [bizproc.task.delegate](./bizproc-task-delegate.md) | Delegates a workflow task ||
|#
