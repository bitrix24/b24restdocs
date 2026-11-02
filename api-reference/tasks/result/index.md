# Task Results: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A task result is the outcome of the work, for example, "The client signed the documents." Bitrix24 shows the result in a separate block of the task card, so you don't need to look for it among the comments. A task can have multiple results.

In the previous API version, a result is a task comment marked as the result. In [REST 3.0](../../rest-v3.md), a result is created from text or from a task chat message, and it is updated and deleted with separate methods.

> Quick access: [all methods](#all-methods)
>
> User documentation: [Task status summary](https://helpdesk.bitrix24.com/open/25869151/)

## Getting Started

Methods of the previous version are called at `/rest/`, REST 3.0 methods at `/rest/api/`. If the [new task card](../tasks-new.md) is enabled in Bitrix24, use REST 3.0 methods: starting from module version `tasks 25.700.0`, comments in the new card became chat messages, and methods of the previous version do not work with them.

### If You Work with the Previous API Version

1. Get the task identifier using [tasks.task.list](../tasks-task-list.md)
2. Create a comment using [task.commentitem.add](../comment-item/task-comment-item-add.md). The method returns the comment identifier
3. Mark the comment as a result using [tasks.task.result.addFromComment](./tasks-task-result-add-from-comment.md)
4. Get the result list using [tasks.task.result.list](./tasks-task-result-list.md)
5. If the result is no longer needed, remove the comment from results using [tasks.task.result.deleteFromComment](./tasks-task-result-delete-from-comment.md)

### If You Work with REST 3.0

1. Get the task identifier using [tasks.task.list](../tasks-task-list-rest-v3.md)
2. Add a result using [tasks.task.result.add](./tasks-task-result-add.md) or create it from a chat message using [tasks.task.result.addfromchatmessage](./tasks-task-result-addfromchatmessage.md). How to retrieve the message identifier is described below, under "Task Chat"
3. If necessary, update the result text using [tasks.task.result.update](./tasks-task-result-update.md)
4. Get the result list using [tasks.task.result.list](./tasks-task-result-list-rest-v3.md)
5. If the result is no longer needed, delete it using [tasks.task.result.delete](./tasks-task-result-delete.md)

### Mapping Previous Version Methods to REST 3.0

If your integration already works with results through comments, replace the methods according to the table.

#|
|| **Previous version method** | **REST 3.0 method** ||
|| [tasks.task.result.addFromComment](./tasks-task-result-add-from-comment.md) | [tasks.task.result.addfromchatmessage](./tasks-task-result-addfromchatmessage.md) — from a task chat message, [tasks.task.result.add](./tasks-task-result-add.md) — from text ||
|| [tasks.task.result.list](./tasks-task-result-list.md) | [tasks.task.result.list](./tasks-task-result-list-rest-v3.md) ||
|| [tasks.task.result.deleteFromComment](./tasks-task-result-delete-from-comment.md) | [tasks.task.result.delete](./tasks-task-result-delete.md) ||
|| No equivalent | [tasks.task.result.update](./tasks-task-result-update.md) ||
|#

The previous list method returns all results at once as an array in `result`. The REST 3.0 method returns them page by page in `result.items`, and the page is set by the `pagination` parameter.

## Linking Results to Other Objects

**Task.** Results are linked to the task by the identifier `taskId`. This can be obtained using the [create new task](../tasks-task-add.md) method or the [get task list](../tasks-task-list.md) method.

**Comment.** In the previous version, [tasks.task.result.addFromComment](./tasks-task-result-add-from-comment.md) creates a result by the comment identifier `commentId`. It is returned by the [create comment](../comment-item/task-comment-item-add.md) and [get comment list](../comment-item/task-comment-item-get-list.md) methods.

**Task Chat.** In REST 3.0, [tasks.task.result.addfromchatmessage](./tasks-task-result-addfromchatmessage.md) creates a result from a message by its identifier `messageId`. You can retrieve `messageId` in two ways:

- using [im.dialog.messages.get](../../chats/messages/im-dialog-messages-get.md) with `DIALOG_ID` in the format `chat{chatId}`, where `chatId` is the `result.item.chatId` field of the [tasks.task.get](../tasks-task-get-rest-v3.md) response. Message identifiers are returned in `result.messages[].id`. The call requires the `im` scope
- from the `data.FIELDS_AFTER.MESSAGE_ID` field of the [OnTaskCommentAdd](../comment-item/events-comment/on-task-comment-add.md) event when a new message arrives in the task chat

The [tasks.task.chat.message.send](../tasks-task-chat-message-send.md) method does not return `messageId`, so after sending a message, retrieve its identifier in one of these ways.

{% note tip "User Documentation" %}

- [Bitrix24 Tasks](https://helpdesk.bitrix24.com/open/18034564/)

{% endnote %}

## How to Delete a Comment

The [tasks.task.result.deleteFromComment](./tasks-task-result-delete-from-comment.md) method does not delete the comment; it only unmarks it as the result. To delete the comment itself, use the [task.commentitem.delete](../comment-item/task-comment-item-delete.md) method.

## Overview of Methods {#all-methods}

### Previous Version Methods

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the methods:
> - any user with access to the task — to retrieve the list
> - the comment author or an administrator with access to the task — to mark a comment as the result and to unmark it

#|
|| **Method** | **Description** ||
|| [tasks.task.result.addFromComment](./tasks-task-result-add-from-comment.md) | Marks a comment as the result ||
|| [tasks.task.result.list](./tasks-task-result-list.md) | Retrieves the list of task results ||
|| [tasks.task.result.deleteFromComment](./tasks-task-result-delete-from-comment.md) | Unmarks a comment as the result ||
|#

### REST 3.0 Methods

> Scope: [`tasks`](../../scopes/permissions.md)
>
> The `tasks` scope is marked as deprecated in the [scope list](../../scopes/permissions.md), but REST 3.0 methods work only with it
>
> Who can execute the methods:
> - any user with access to the task — to add a result and retrieve the list
> - the result author — to update a result
> - the result author or an administrator — to delete a result

#| 
|| **Method** | **Description** || 
|| [tasks.task.result.add](./tasks-task-result-add.md) | Adds a result to the task ||
|| [tasks.task.result.addfromchatmessage](./tasks-task-result-addfromchatmessage.md) | Creates a result from a task chat message ||
|| [tasks.task.result.update](./tasks-task-result-update.md) | Updates the result text ||
|| [tasks.task.result.list](./tasks-task-result-list-rest-v3.md) | Retrieves a list of task results v 3.0 ||
|| [tasks.task.result.delete](./tasks-task-result-delete.md) | Deletes a task result ||
|#

The `tasks.task.result.list` method exists in two versions, [previous](./tasks-task-result-list.md) and [REST 3.0](./tasks-task-result-list-rest-v3.md), so in the REST 3.0 table it is marked `v 3.0`.
