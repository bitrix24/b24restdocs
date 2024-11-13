# Task Results: Overview of Methods

The result of a task is a fixed comment summarizing the work done on the task. The result is highlighted in a separate block within the task card, so it doesn't need to be searched among all comments. A task can have multiple results.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [how to fix the result of the task](https://helpdesk.bitrix24.com/open/21841518/) 

## Linking Results to Other Objects

**Task.** Results are linked to the task by the identifier `taskId`. This can be obtained using the [create new task](../tasks-task-add.md) method or the [get task list](../tasks-task-list.md) method.

**Comment.** The task result is created from a comment using the identifier `commentId`. The comment identifier can be obtained using the [create comment](../comment-item/task-comment-item-add.md) method or the [get comment list](../comment-item/task-comment-item-get-list.md) method for the task.

{% note tip "User Documentation" %}

- [Bitrix24 Tasks](https://helpdesk.bitrix24.com/open/18034564/)

{% endnote %}

## How to Delete a Comment

The [tasks.task.result.deleteFromComment](./tasks-task-result-delete-from-comment.md) method does not delete the comment; it only removes its fixation as a result. To delete a comment with a result, use the [task.commentitem.delete](../comment-item/task-comment-item-delete.md) method.

## Overview of Methods {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#| 
|| **Method** | **Description** ||
|| [tasks.task.result.addFromComment](./tasks-task-result-add-from-comment.md) | Adds a comment to the result ||
|| [tasks.task.result.list](./tasks-task-result-list.md) | Retrieves the list of task results ||
|| [tasks.task.result.deleteFromComment](./tasks-task-result-delete-from-comment.md) | Deletes a comment from the task result ||
|#