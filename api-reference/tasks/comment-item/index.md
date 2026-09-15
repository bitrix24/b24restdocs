# Comments in Tasks: Overview of Methods and Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

In comments, participants discuss the work on a task, ask questions, and record the result. In the old task card, a comment is a message of the task forum. The rest of the task methods are collected in the [Tasks](../index.md) section.

{% note warning "Comments Are Replaced with a Chat in the New Task Card" %}

The new task card is available starting with module version `tasks 25.700.0`. In it, a comment is retained as a chat message, so some methods of the section do not work. Starting with the same version, development has stopped for the methods that create, read, update, and delete comments: use the replacement methods in new integrations. What to use instead of each method is described below.

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Working with tasks](https://helpdesk.bitrix24.com/open/16801398)

## Method Status in the New Task Card {#status}

#|
|| **Operation** | **Method of the Section** | **Status in the New Card** | **Replacement** ||
|| Add a comment | [task.commentitem.add](./task-comment-item-add.md) | Works, but takes only `POST_MESSAGE` and `AUTHOR_ID` into account | For new integrations — [tasks.task.chat.message.send](../tasks-task-chat-message-send.md) ||
|| Update a comment | [task.commentitem.update](./task-comment-item-update.md) | Does not work | [im.message.update](../../chats/messages/im-message-update.md) ||
|| Retrieve a comment | [task.commentitem.get](./task-comment-item-get.md) | Does not work | [im.dialog.messages.get](../../chats/messages/im-dialog-messages-get.md) ||
|| Retrieve a list of comments | [task.commentitem.getlist](./task-comment-item-get-list.md) | Does not work | [im.dialog.messages.get](../../chats/messages/im-dialog-messages-get.md) ||
|| Delete a comment | [task.commentitem.delete](./task-comment-item-delete.md) | Does not work | [im.message.delete](../../chats/messages/im-message-delete.md) ||
|| Check the rights to an action with a comment | [task.commentitem.isactionallowed](./task-comment-item-is-action-allowed.md) | Works, but only with the comments of the old card | There is no direct replacement: the rights to actions with a message are checked by the methods of the [Chats](../../chats/index.md) section ||
|| Retrieve the description of the section's methods | [task.commentitem.getmanifest](./task-comment-item-get-manifest.md) | Works | No replacement needed ||
|#

In the new card, the [OnTaskCommentAdd](./events-comment/on-task-comment-add.md) event returns `ID = 0`, and the chat message identifier comes in `MESSAGE_ID`.

The replacement methods work in other scopes: the `im.*` chat methods require the `im` scope, and the [tasks.task.chat.message.send](../tasks-task-chat-message-send.md) method requires the `tasks` scope. The latter belongs to [REST 3.0](../../rest-v3.md) and is called at `/rest/api/`. The `DIALOG_ID` format and the migration of the other task sections are described in the article [{#T}](../tasks-new.md).

## Connection of Comments with Other Objects

A comment is linked to a task, a user, Drive files, and the task result.

**Task.** Comments are linked to the task by the identifier `TASKID`. You can retrieve it using the [tasks.task.add](../tasks-task-add.md) or [tasks.task.list](../tasks-task-list.md) method.

**Comment.** The comment itself is addressed by the identifier `ITEMID`. It is returned by the [task.commentitem.add](./task-comment-item-add.md) method, and you can find a comment among the existing ones using the [task.commentitem.getlist](./task-comment-item-get-list.md) method.

**User.** A comment is linked to its author by the numeric identifier `AUTHOR_ID`. You can retrieve the user identifier using the [user.get](../../user/user-get.md) method.

**Drive Files.** In the old card, you can attach Drive files to a comment through the `UF_FORUM_MESSAGE_DOC` parameter — an array of strings with the prefix `n` before the file identifier, for example `["n4755", "n4753"]`. The file identifier is returned by the upload methods [disk.storage.uploadfile](../../disk/storage/disk-storage-upload-file.md) and [disk.folder.uploadfile](../../disk/folder/disk-folder-upload-file.md), or by the methods that retrieve a list of files, [disk.storage.getchildren](../../disk/storage/disk-storage-get-children.md) and [disk.folder.getchildren](../../disk/folder/disk-folder-get-children.md).

In the new card, a file is sent to the task chat using the [im.v2.File.upload](../../chat-bots/chat-bots-v2/im.v2/files/file-upload.md) method: it uploads the file and sends a message in a single call. If the file is already on Drive, use [im.disk.file.commit](../../chats/files/im-disk-file-commit.md).

**Task Result.** A comment can be pinned as the result of task execution with the group of methods [tasks.task.result.*](../result/index.md). In the old card, the [tasks.task.result.deleteFromComment](../result/tasks-task-result-delete-from-comment.md) method only unpins the result and the comment itself remains; in the new card, this method does not work. Deleting a comment also deletes the result linked to it.

{% note tip "Typical use-cases and scenarios" %}

- [{#T}](../../../tutorials/tasks/how-to-create-comment-with-file.md)

{% endnote %}

## Getting Started

The new card is enabled on the Bitrix24 account, so the order of actions depends on which card is active. The status of each method is given in the table [Method Status in the New Task Card](#status).

In the old card:

1. Retrieve the task identifier `TASKID`
2. Add a comment using the [task.commentitem.add](./task-comment-item-add.md) method and retain the `ITEMID` from the response
3. Update or delete the comment by `TASKID` and `ITEMID` using the [task.commentitem.update](./task-comment-item-update.md) and [task.commentitem.delete](./task-comment-item-delete.md) methods

In the new card, the work goes through the task chat: request the `CHAT_ID` field of the task using the [tasks.task.get](../tasks-task-get.md) method — the value comes in the `chatId` field — and then use the methods from the Replacement column of the status table.

## Who Can Work with Comments

The rights apply to the methods of this section.

- a user with the read access right to the task or higher can add a comment, retrieve it by identifier, and retrieve the list of comments
- an administrator can update and delete a comment, and so can the author of the comment — but only if it is the last comment in the task. Both operations can be disabled in the settings of the task module: in that case, the method returns an error even for an administrator
- any user can check the rights to an action and retrieve the description of the methods

## Specifics of the Section's Methods

- in the new card, the methods marked as not working in the status table behave differently: [task.commentitem.getlist](./task-comment-item-get-list.md) returns an empty list without an error, [task.commentitem.get](./task-comment-item-get.md) returns an error, and [task.commentitem.update](./task-comment-item-update.md) and [task.commentitem.delete](./task-comment-item-delete.md) return the error `4/TE/ACTION_NOT_ALLOWED`. These signs show that the task comments have already moved to the chat
- in the `get`, `getlist`, `update`, and `delete` methods, parameters are passed positionally: the key names stay as usual, but their order has to match the order in the parameter table on the method page, otherwise the request returns an error
- the methods for working with comments, except for `getmanifest`, return errors with the `ERROR_CORE` code, and the cause differs in the text of `error_description`. The machine-readable part of the text is a construction of the form `TASKS_ERROR_EXCEPTION_#4; Action is not allowed; 4/TE/ACTION_NOT_ALLOWED`. Typical causes are an invalid parameter type, a missing required parameter, and a lack of rights to the task. The general error format is described in the article [Error Codes](../../../error-codes.md), and the codes of a specific method are in the Error Handling section on its page. The replacement methods from the [Chats](../../chats/index.md) section have their own error model and their own response format
- comment fields are returned in uppercase, and numeric identifiers such as `ID` and `AUTHOR_ID` come as strings. This applies to the methods that return a comment. The `add` method responds with a number, and `isactionallowed` with a boolean value. Attachments are placed in the `ATTACHED_OBJECTS` object, where the key is the attachment identifier. The full response structure and field types are described in the Response Handling section on the method pages

## Overview of Methods and Events {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

{% list tabs %}

- Methods

    The status of each method is given in the table [Method Status in the New Task Card](#status).

    #|
    || **Method** | **Description** ||
    || [task.commentitem.add](./task-comment-item-add.md) | Adds a comment to a task ||
    || [task.commentitem.update](./task-comment-item-update.md) | Updates a comment ||
    || [task.commentitem.get](./task-comment-item-get.md) | Retrieves a task comment by `ITEMID` ||
    || [task.commentitem.getlist](./task-comment-item-get-list.md) | Retrieves a list of comments for a task ||
    || [task.commentitem.delete](./task-comment-item-delete.md) | Deletes a comment ||
    || [task.commentitem.isactionallowed](./task-comment-item-is-action-allowed.md) | Checks whether an action with a comment is allowed ||
    || [task.commentitem.getmanifest](./task-comment-item-get-manifest.md) | Returns the list of the section's methods and their description ||
    |#

- Events

    How to subscribe to events and receive them is described in the section [{#T}](./events-comment/index.md).

    #|
    || **Event** | **Triggered** ||
    || [OnTaskCommentAdd](./events-comment/on-task-comment-add.md) | When a comment is added to a task. Triggered in the new card ||
    || [OnTaskCommentUpdate](./events-comment/on-task-comment-update.md) | When a comment is updated in a task. Not triggered in the new card ||
    || [OnTaskCommentDelete](./events-comment/on-task-comment-delete.md) | When a comment is deleted from a task. Not triggered in the new card ||
    |#

{% endlist %}
