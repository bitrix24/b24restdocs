# Workflow Tasks: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Workflows can create tasks for participants to obtain a decision or data from them. For example, a task can be used to approve a document in accounting, authorize a vacation with a manager, or request a contract from a lawyer. Users can see their tasks in Bitrix24 and receive notifications.

The [bizproc.task.complete](./bizproc-task-complete.md) method completes four types of tasks:

- Document approval
- Document review
- Request for additional information
- Request for additional information with rejection

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Workflow Assignments](https://helpdesk.bitrix24.com/open/11466058/)

## How to Get Started

1. Retrieve the list of tasks using the [bizproc.task.list](./bizproc-task-list.md) method. The method returns an array of tasks; the default fields include `ID`, `WORKFLOW_ID`, `DOCUMENT_NAME`, and `NAME`
2. Pick the identifier of the required task `ID`: to complete the task, pass it in `TASK_ID`; to delegate it, pass it in the `TASK_IDS` array
3. Complete the task using the [bizproc.task.complete](./bizproc-task-complete.md) method or delegate it to another employee using the [bizproc.task.delegate](./bizproc-task-delegate.md) method. Both methods return `true` on success

## Task Fields

Tasks that request additional information are completed using the [bizproc.task.complete](./bizproc-task-complete.md) method. Field values are passed in the `FIELDS` object in the format `{"field_1": "value_1", ... , "field_N": "value_N"}`:

- `field_N` — the `Id` value of a field from `PARAMETERS.Fields`
- `value_N` — value of the field

You can find out which fields must be filled in using the [bizproc.task.list](./bizproc-task-list.md) method: specify `PARAMETERS` in the `SELECT` parameter, and the field descriptions are returned in `PARAMETERS.Fields`. For other task types, the response contains no `Fields`.

## What to Consider

- You can complete only your own tasks
- You can delegate your own tasks and the tasks of your subordinates; an administrator can delegate the tasks of any employee. Only a company employee can delegate and receive a task
- The [bizproc.task.list](./bizproc-task-list.md) method returns tasks of all types, for example, RPA tasks. The task type is in the `ACTIVITY` field. For an unsupported type, the [bizproc.task.complete](./bizproc-task-complete.md) method returns the `ERROR_TASK_TYPE` error
- The methods are available if the Bitrix24 plan includes workflows

## Relationship with Other Objects

**User.** A task is linked to an assignee. You can retrieve employee identifiers for delegation using the [user.get](../../user/user-get.md) method.

**Workflow.** A task is created while a workflow is running. The workflow identifier is returned in the task's `WORKFLOW_ID` field. You can use it to find the workflow with the [bizproc.workflow.instances](../bizproc-workflow-instances.md) method or to stop it with the [bizproc.workflow.terminate](../bizproc-workflow-terminate.md) method. Other methods of the section are listed in the [workflows overview](../index.md).

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
