# Comments in Tasks: Overview of Methods

In task comments, participants ask questions, discuss the task, and track progress. Important notes can be left here, and results can be recorded.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [Working with tasks](https://helpdesk.bitrix24.com/open/16801398)

## Connection of Comments with Other Objects

**Task.** Comments are linked to the task by the identifier `TASKID`. You can obtain it using the [create new task](../tasks-task-add.md) method or the [get task list](../tasks-task-list.md) method.

**User.** A comment is associated with a user by the numeric identifier in the `AUTHOR_ID` parameter. You can get the user identifier using the [user.get](../../user/user-get.md) method.

{% note tip "User documentation" %}

- [Tasks in Bitrix24](https://helpdesk.bitrix24.com/open/18034564/)

{% endnote %}

## Drive Files

You can attach Drive files to a task comment. In the `UF_FORUM_MESSAGE_DOC` parameter, pass an array with the identifiers of the Drive files. Prefix each identifier with `n`, for example: `"UF_FORUM_MESSAGE_DOC": ["n428", "n345"]`. You can obtain file identifiers in two ways.

Use one of the file upload methods:
  - [disk.storage.uploadfile](../../disk/storage/disk-storage-upload-file.md)
  - [disk.folder.uploadfile](../../disk/folder/disk-folder-upload-file.md)

Use one of the methods to get the list of files:
  - [disk.storage.getchildren](../../disk/storage/disk-storage-get-children.md)
  - [disk.folder.getchildren](../../disk/folder/disk-folder-get-children.md)

{% note tip "Typical use-cases and scenarios" %}

- [{#T}](../../../tutorials/tasks/how-to-create-comment-with-file.md)

{% endnote %}

## Task Execution Results

A comment can be pinned as the result of task execution. Manage task results with the group of methods [tasks.task.result.*](../result/index.md).

The method [task.commentitem.delete](./task-comment-item-delete.md) will delete the comment with the result. 

## Overview of Methods and Events {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [task.commentitem.add](./task-comment-item-add.md) | Add a comment to a task ||
    || [task.commentitem.update](./task-comment-item-update.md) | Update a comment ||
    || [task.commentitem.get](./task-comment-item-get.md) | Get a task comment by `id` ||
    || [task.commentitem.getlist](./task-comment-item-get-list.md) | Get a list of comments for a task ||
    || [task.commentitem.delete](./task-comment-item-delete.md) | Delete a comment ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [OnTaskCommentAdd](./events-comment/on-task-comment-add.md) | When a comment is added to a task ||
    || [OnTaskCommentUpdate](./events-comment/on-task-comment-update.md) | When a comment is updated in a task ||
    || [OnTaskCommentDelete](./events-comment/on-task-comment-delete.md) | When a comment is deleted from a task ||
    |#

{% endlist %}