# Task Results: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The result of a task is a fixed comment summarizing the work done on the task. The result is highlighted in a separate block within the task card, so it doesn't need to be searched among all comments. A task can have multiple results.

In [REST 3.0](../../rest-v3.md), task results have separate methods for creating, updating, and deleting a result, as well as a list method with the new response format.

> Quick access: [all methods](#all-methods)
> 
> User documentation: [how to fix the result of work on a task](https://helpdesk.bitrix24.com/open/21841518/) 

## Getting Started

### If You Work with the Previous Version

1. Get the task identifier
2. Create a comment using [task.commentitem.add](../comment-item/task-comment-item-add.md)
3. Mark the comment as a result using [tasks.task.result.addFromComment](./tasks-task-result-add-from-comment.md)
4. Get the result list using [tasks.task.result.list](./tasks-task-result-list.md)
5. If the result is no longer needed, remove the comment from results using [tasks.task.result.deleteFromComment](./tasks-task-result-delete-from-comment.md)

### If You Work with REST 3.0

1. Get the task identifier
2. Add a result using [tasks.task.result.add](./tasks-task-result-add.md) or create it from a chat message using [tasks.task.result.addfromchatmessage](./tasks-task-result-addfromchatmessage.md)
3. If necessary, update the result text using [tasks.task.result.update](./tasks-task-result-update.md)
4. Get the result list using [tasks.task.result.list](./tasks-task-result-list-rest-v3.md)
5. If the result is no longer needed, delete it using [tasks.task.result.delete](./tasks-task-result-delete.md)

## Linking Results to Other Objects

**Task.** Results are linked to the task by the identifier `taskId`. This can be obtained using the [create new task](../tasks-task-add.md) method or the [get task list](../tasks-task-list.md) method.

**Comment.** The task result is created from a comment by the identifier `commentId`. The comment identifier can be obtained using the [create comment](../comment-item/task-comment-item-add.md) method or the [get comment list](../comment-item/task-comment-item-get-list.md) method for the task.

**Task Chat.** In REST 3.0, [tasks.task.result.addfromchatmessage](./tasks-task-result-addfromchatmessage.md) creates a result from a task chat message. To do this, pass the `messageId` obtained when sending a message using [tasks.task.chat.message.send](../tasks-task-chat-message-send.md) or the [chat message methods](../../chats/messages/index.md).

{% note tip "User Documentation" %}

- [Bitrix24 Tasks](https://helpdesk.bitrix24.com/open/18034564/)

{% endnote %}

## How to Delete a Comment

The [tasks.task.result.deleteFromComment](./tasks-task-result-delete-from-comment.md) method does not delete the comment; it only removes its fixation as a result. To delete a comment with the result, use the [task.commentitem.delete](../comment-item/task-comment-item-delete.md) method.

## Overview of Methods {#all-methods}

> Scope:
>
> - [`task`](../../scopes/permissions.md) — for methods of the previous API version
> - [`tasks`](../../scopes/permissions.md) — for REST 3.0 methods
>
> Who can execute the method: depends on the method

#| 
|| **Method** | **Description** || 
|| [tasks.task.result.add](./tasks-task-result-add.md) | Adds a result to the task ||
|| [tasks.task.result.addfromchatmessage](./tasks-task-result-addfromchatmessage.md) | Creates a result from a task chat message ||
|| [tasks.task.result.update](./tasks-task-result-update.md) | Updates the result text ||
|| [tasks.task.result.list](./tasks-task-result-list-rest-v3.md) | Gets a list of task results v 3.0 ||
|| [tasks.task.result.delete](./tasks-task-result-delete.md) | Deletes a task result ||
|| [tasks.task.result.addFromComment](./tasks-task-result-add-from-comment.md) | Adds a comment to the result || 
|| [tasks.task.result.list](./tasks-task-result-list.md) | Retrieves the list of task results || 
|| [tasks.task.result.deleteFromComment](./tasks-task-result-delete-from-comment.md) | Removes a comment from the task result || 
|#
