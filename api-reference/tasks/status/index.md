# Task Status Changes: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods in this section move a task between its main working statuses: they let you start work, pause work, defer a task, complete it, and return it to work after completion.

The task status reflects the current stage of work and affects the task card, change history, and actions available to participants.

> Quick links: [all methods](#all-methods)
>
> User documentation: [Track employee tasks in Bitrix24](https://helpdesk.bitrix24.com/open/27774804/)

## Getting Started

1. Create a task using [tasks.task.add](../tasks-task-add.md) or get its identifier using [tasks.task.list](../tasks-task-list.md)
2. Get current task data using [tasks.task.get](../tasks-task-get.md) if you need to check the current status and available actions
3. Start work using [tasks.task.start](./tasks-task-start.md) when the assignee begins execution
4. Pause work using [tasks.task.pause](./tasks-task-pause.md) if the task needs to be stopped temporarily
5. Defer the task using [tasks.task.defer](./tasks-task-defer.md) if you need to return to it later
6. Complete the task using [tasks.task.complete](./tasks-task-complete.md) when the work is done
7. Renew the task using [tasks.task.renew](./tasks-task-renew.md) if it needs to be returned to work after completion

## Relation to Other Objects

**Task.** All methods in this section work with an existing task by its identifier. Create a task using [tasks.task.add](../tasks-task-add.md), get task data using [tasks.task.get](../tasks-task-get.md), and get a task list using [tasks.task.list](../tasks-task-list.md).

**Users.** The ability to change status depends on the user's role in the task and access permissions. You can check access to the task using [tasks.task.getaccess](../tasks-task-get-access.md).

**Task History.** Status changes are recorded in the task history. You can get the change history using [tasks.task.history.list](../tasks-task-history-list.md).

**Completion Control.** If completion control is enabled in the task, after the task is moved to Completed, the creator can accept the result using [tasks.task.approve](../tasks-task-approve.md) or return the task to work using [tasks.task.disapprove](../tasks-task-disapprove.md).

## Overview of Methods {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the user's role in the task

#|
|| **Method** | **Description** ||
|| [tasks.task.start](./tasks-task-start.md) | Moves a task to In Progress status ||
|| [tasks.task.pause](./tasks-task-pause.md) | Stops task execution and moves it to Waiting status ||
|| [tasks.task.defer](./tasks-task-defer.md) | Moves a task to Deferred status ||
|| [tasks.task.complete](./tasks-task-complete.md) | Moves a task to Completed status ||
|| [tasks.task.renew](./tasks-task-renew.md) | Renews a task after completion ||
|#
