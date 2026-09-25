# Time Tracking in Tasks: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

With the Time Tracking in Tasks tool, you can:

- monitor the time spent by an employee on specific tasks
- enhance the transparency of task execution
- analyze employee efficiency

> Quick Navigation: [all methods](#all-methods)
>
> User Documentation: [Time Tracking in Tasks](https://helpdesk.bitrix24.com/open/25873619/)

## Linking Time Spent with Other Objects

**Task.** A single task can have multiple entries for time spent. These entries are linked by the task identifier `TASKID`. You can obtain it using the [create new task](../tasks-task-add.md) method or the [get task list](../tasks-task-list.md) method.

**User.** The time spent entry is associated with a user by their identifier `USER_ID`. You can get the user identifier using the [user.get](../../user/user-get.md) method.

{% note tip "User Documentation" %}

- [Bitrix24 tasks](https://helpdesk.bitrix24.com/open/18034564/)

{% endnote %}

## Time Entry Data

A time entry stores the task and author identifiers, work duration, a comment, and the creation date. The field names differ between the classic `task.elapseditem.*` methods and REST 3.0.

#|
|| **Data** | **`task.elapseditem.*` Field** | **REST 3.0 Field** ||
|| Entry identifier | `ID` | `id` ||
|| Task identifier | `TASK_ID` | `taskId` ||
|| Entry author | `USER_ID` | `userId` ||
|| Time spent | `SECONDS`, `MINUTES` | `seconds`, `minutes` ||
|| Comment | `COMMENT_TEXT` | `text` ||
|| Creation date | `CREATED_DATE` | `createdAtTs` ||
|#

In REST 3.0, you can retrieve time tracking data together with the task using [tasks.task.get](../tasks-task-get-rest-v3.md). Pass the required nested `elapsedTime` fields in the `select` parameter, for example, `elapsedTime.minutes`, `elapsedTime.text`, `elapsedTime.createdAtTs`, and `elapsedTime.userId`. The complete set of fields is provided in the [time tracking object](../fields-rest-v3.md#elapsed-time) description.

## How to Get Started

1. Add an entry using [task.elapseditem.add](./task-elapsed-item-add.md)
2. Retrieve the task entries using [task.elapseditem.getlist](./task-elapsed-item-get-list.md)
3. Before updating or deleting an entry, check whether the action is available using [task.elapseditem.isactionallowed](./task-elapsed-item-is-action-allowed.md), then call [task.elapseditem.update](./task-elapsed-item-update.md) or [task.elapseditem.delete](./task-elapsed-item-delete.md)

## Who Can Add or Modify an Entry

To add, modify, or delete a time spent entry, you need access permissions to the task. You can check permissions using the special method [task.elapseditem.isactionallowed](./task-elapsed-item-is-action-allowed.md).

## Reference Information on Methods

You can find up-to-date information about methods for working with elapsed time using the [task.elapseditem.getmanifest](./task-elapsed-item-get-manifest.md) method. Use it only as a reference: the response structure may change at any time.

## Overview of Methods {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

#| 
|| **Method** | **Description** ||
|| [task.elapseditem.add](./task-elapsed-item-add.md) | Adds time spent to a task ||
|| [task.elapseditem.update](./task-elapsed-item-update.md) | Modifies the parameters of a time spent entry ||
|| [task.elapseditem.get](./task-elapsed-item-get.md) | Returns a time spent entry by its identifier ||
|| [task.elapseditem.getlist](./task-elapsed-item-get-list.md) | Returns a list of time spent entries for a task ||
|| [task.elapseditem.delete](./task-elapsed-item-delete.md) | Deletes a time spent entry ||
|| [task.elapseditem.isactionallowed](./task-elapsed-item-is-action-allowed.md) | Checks whether the action is allowed ||
|| [task.elapseditem.getmanifest](./task-elapsed-item-get-manifest.md) | Returns a list of methods and their descriptions ||
|#
