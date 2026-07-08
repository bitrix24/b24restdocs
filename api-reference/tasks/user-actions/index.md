# User Actions on Task: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods in this section help manage personal task settings: start watching a task, add it to favorites, pin it in the list, and enable silent mode.

These methods do not change the task content. They help track updates, find required tasks faster in the list, and disable unnecessary notifications.

> Quick links: [all methods](#all-methods)
> 
> User documentation: [How to find favorite tasks](https://helpdesk.bitrix24.com/open/27648776/#4)

## Getting Started

1. Get the task identifier using [tasks.task.list](../tasks-task-list.md) or [tasks.task.get](../tasks-task-get.md)
2. Start watching the task using [tasks.task.startwatch](./tasks-task-start-watch.md) if you want to receive task updates
3. Stop watching the task using [tasks.task.stopwatch](./tasks-task-stop-watch.md) if you no longer need to track it
4. Add the task to favorites using [tasks.task.favorite.add](./tasks-task-favorite-add.md) to find it faster in the list
5. Remove the task from favorites using [tasks.task.favorite.remove](./tasks-task-favorite-remove.md) if it is no longer needed in quick access
6. Pin the task using [tasks.task.pin](./tasks-task-pin.md) if you want to keep it fixed in the list
7. Unpin the task using [tasks.task.unpin](./tasks-task-unpin.md) if pinning is no longer required
8. Enable silent mode using [tasks.task.mute](./tasks-task-mute.md) if you do not need task notifications
9. Disable silent mode using [tasks.task.unmute](./tasks-task-unmute.md) to receive notifications again

## Relation to Other Objects

**Task.** All methods in this section work with an existing task by its identifier. You can get the task identifier and data using [tasks.task.get](../tasks-task-get.md) and [tasks.task.list](../tasks-task-list.md).

**User.** The methods perform actions of the current user on the task. The ability to perform an action depends on task access. You can check access using [tasks.task.getaccess](../tasks-task-get-access.md).

**Task List.** Pinning and favorites help find a task faster in the common list, while silent mode and watching control how you work with task notifications.

## Overview of Methods {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: user with access to the task

#|
|| **Method** | **Description** ||
|| [tasks.task.startwatch](./tasks-task-start-watch.md) | Starts watching a task ||
|| [tasks.task.stopwatch](./tasks-task-stop-watch.md) | Stops watching a task ||
|| [tasks.task.favorite.add](./tasks-task-favorite-add.md) | Adds a task to favorites ||
|| [tasks.task.favorite.remove](./tasks-task-favorite-remove.md) | Removes a task from favorites ||
|| [tasks.task.pin](./tasks-task-pin.md) | Pins a task in the list ||
|| [tasks.task.unpin](./tasks-task-unpin.md) | Unpins a task in the list ||
|| [tasks.task.mute](./tasks-task-mute.md) | Enables silent mode ||
|| [tasks.task.unmute](./tasks-task-unmute.md) | Disables silent mode ||
|#
