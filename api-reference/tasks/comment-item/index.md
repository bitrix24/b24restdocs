# Comments in Tasks: Overview of Methods

In task comments, participants ask questions, discuss the task, and track progress. Important notes can be left here, and results can be recorded.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [Working with tasks](https://helpdesk.bitrix24.com/open/16801398)

## Connection of Comments with Other Objects

**Task.** Comments are linked to the task by the identifier `TASKID`. You can obtain it using the [create new task](../tasks-task-add.md) method or the [get task list](../tasks-task-list.md) method.

**User.** The comment is associated with a user by the numerical identifier in the `AUTHOR_ID` parameter. You can get the user identifier using the [user.get](../../user/user-get.md) method.

{% note tip "User Documentation" %}

- [Tasks in Bitrix24](https://helpdesk.bitrix24.com/open/18034564/)

{% endnote %}

## Drive Files

You can attach Drive files to the task comment. In the `UF_FORUM_MESSAGE_DOC` parameter, pass an array with the identifiers of the Drive files. Prefix each identifier with `n`, for example: `"UF_FORUM_MESSAGE_DOC": ["n428", "n345"]`. You can obtain file identifiers in two ways.

Use one of the file upload methods:
  - [disk.storage.uploadfile](../../disk/storage/disk-storage-upload-file.md)
  - [disk.folder.uploadfile](../../disk/folder/disk-folder-upload-file.md)

Use one of the methods to get the list of files:
  - [disk.storage.getchildren](../../disk/storage/disk-storage-get-children.md)
  - [disk.folder.getchildren ](../../disk/folder/disk-folder-get-children.md)

{% note tip "Typical use-cases and scenarios" %}

- [{#T}](../../../tutorials/tasks/how-to-create-comment-with-file.md)

{% endnote %}

## Who Can Add or Change a Comment

To add, modify, or delete a comment, you need access permissions to the task and the comment. You can check permissions using the [task.commentitem.isactionallowed](./task-comment-item-is-action-allowed.md) method.

## Reference Information on Methods

You can find up-to-date information about methods for working with task comments using the [task.commentitem.getmanifest](./task-comment-item-get-manifest.md) method. It is recommended to use it only as a reference, as the response structure may change at any time.

## Task Execution Results

A comment can be pinned as a result of task execution. Manage task results with the group of methods [tasks.task.result.*](../result/index.md).

The [task.commentitem.delete](./task-comment-item-delete.md) method will delete the comment with the result. 

## Overview of Methods and Events {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [task.commentitem.add](./task-comment-item-add.md) | Add a comment to the task ||
    || [task.commentitem.update](./task-comment-item-update.md) | Update a comment ||
    || [task.commentitem.get](./task-comment-item-get.md) | Get a task comment by `id` ||
    || [task.commentitem.getlist](./task-comment-item-get-list.md) | Get a list of comments for the task ||
    || [task.commentitem.delete](./task-comment-item-delete.md) | Delete a comment ||
    || [task.commentitem.isactionallowed](./task-comment-item-is-action-allowed.md) | Check if the action with the comment is allowed ||
    || [task.commentitem.getmanifest](./task-comment-item-get-manifest.md) | Get a list of methods for working with comments and their descriptions ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [OnTaskCommentAdd](./events-comment/on-task-comment-add.md) | When a comment is added to the task ||
    || [OnTaskCommentUpdate](./events-comment/on-task-comment-update.md) | When a comment is updated in the task ||
    || [OnTaskCommentDelete](./events-comment/on-task-comment-delete.md) | When a comment is deleted from the task ||
    |#

{% endlist %}