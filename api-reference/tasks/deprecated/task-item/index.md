# Deprecated task.item.* Methods: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "DEPRECATED" %}

The development of `task.item.*` methods has been stopped. Use [current task methods `tasks.task.*`](../../index.md).

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The `task.item.*` methods are kept only to support legacy integrations. For new development, use the [tasks.task.*](../../index.md) methods, which work with the current task card.

> Quick navigation: [all methods](#all-methods)

## How to Choose a Version

If you are creating a new integration, use the [tasks.task.*](../../index.md) methods. They support current task scenarios, comments, files, results, and statuses.

If an integration already uses `task.item.*`, keep these methods only to maintain existing code. When you update the integration, plan the migration to `tasks.task.*`.

## Relationship with Other Objects

**Task.** The `task.item.*` methods manage a task by its identifier. You can retrieve current task data using [task.item.getdata](./task-item-get-data.md) or through the new [tasks.task.get](../../tasks-task-get.md) branch.

**Files.** Files are attached to a task using the deprecated [task.item.addfile](./task-item-add-file.md) and [task.item.deletefile](./task-item-delete-file.md) methods. In the new branch, use the [tasks.task.files.attach](../../tasks-task-files-attach.md) method.

**Change History.** Task change history is returned by the [task.logitem.list](./task-log-item-list.md) method.

## Overview of Methods {#all-methods}

### Basic Methods

#|
|| **Method** | **Description** ||
|| [task.item.add](./task-item-add.md) | Creates a task ||
|| [task.item.update](./task-item-update.md) | Updates a task ||
|| [task.item.getdata](./task-item-get-data.md) | Returns task data ||
|| [task.item.list](./task-item-list.md) | Returns a list of tasks ||
|| [task.item.delete](./task-item-delete.md) | Deletes a task ||
|| [task.item.getdescription](./task-item-get-description.md) | Returns the task description ||
|| [task.item.getfiles](./task-item-get-files.md) | Returns links to task files ||
|| [task.item.getdependson](./task-item-get-dependson.md) | Returns identifiers of tasks that the task depends on ||
|| [task.logitem.list](./task-log-item-list.md) | Returns the task change history ||
|#

### Reference Methods

#|
|| **Method** | **Description** ||
|| [task.item.getmanifest](./task-item-get-manifest.md) | Returns a list of `task.item.*` methods with their descriptions ||
|| [task.item.getallowedactions](./task-item-get-allowed-actions.md) | Returns identifiers of allowed actions on the task ||
|| [task.item.getallowedtaskactionsasstrings](./task-item-get-allowed-task-actions-as-strings.md) | Returns a list of allowed actions on the task ||
|| [task.item.isactionallowed](./task-item-is-action-allowed.md) | Checks if the action is allowed ||
|#

### Status Management

#|
|| **Method** | **Description** ||
|| [task.item.delegate](./task-item-delegate.md) | Delegates the task to a new user ||
|| [task.item.startexecution](./task-item-start-execution.md) | Changes the task status to "in progress" ||
|| [task.item.defer](./task-item-defer.md) | Changes the task status to "deferred" ||
|| [task.item.complete](./task-item-complete.md) | Changes the task status to "completed" or "conditionally completed" ||
|| [task.item.renew](./task-item-renew.md) | Changes the task status to "not executed" ||
|| [task.item.approve](./task-item-approve.md) | Changes a task awaiting control to "completed" ||
|| [task.item.disapprove](./task-item-disapprove.md) | Changes a task awaiting control to "not executed" ||
|#

### Favorites and Files

#|
|| **Method** | **Description** ||
|| [task.item.addtofavourite](./task-item-add-to-favourite.md) | Adds the task to Favorites ||
|| [task.item.deletefromfavorite](./task-item-delete-from-favorite.md) | Removes the task from Favorites ||
|| [task.item.addfile](./task-item-add-file.md) | Uploads a file to the task ||
|| [task.item.deletefile](./task-item-delete-file.md) | Removes the file attachment from the task ||
|#
