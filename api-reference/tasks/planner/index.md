# Tasks in "Plan for the Day": Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

"Plan for the Day" helps a user collect tasks, activities, and meetings that need to be completed during the working day.

The section method retrieves a list of task IDs from the "Plan for the Day" of the user on whose behalf the request is being made. Using these IDs, you can retrieve detailed task data and build a daily work list within an application.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Planner](https://helpdesk.bitrix24.com/open/24579864/)

## Getting Started

1. Retrieve a list of task IDs in the plan of the user on whose behalf the request is being made using the [task.planner.getList](./task-planner-get-list.md) method.
2. Retrieve detailed data for a specific task using the [tasks.task.get](../tasks-task-get.md) method.
3. If you need to filter tasks by specific conditions, use the [tasks.task.list](../tasks-task-list.md) method.

## Connection with Other Objects

**Task.** The [task.planner.getList](./task-planner-get-list.md) method returns task IDs. Using an ID, you can retrieve task data via the [tasks.task.get](../tasks-task-get.md) method or find tasks by filter via the [tasks.task.list](../tasks-task-list.md) method.

**User.** The method works with the "Plan for the Day" of the user on whose behalf the request is being made. Access to task data depends on the permissions of that user.

## Overview of Methods {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [task.planner.getList](./task-planner-get-list.md) | Retrieves a list of task identifiers from the "Daily Plan" of the user on whose behalf the request is being executed ||
|#
