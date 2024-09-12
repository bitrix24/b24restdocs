# Deprecated Task Methods

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

This section contains information about deprecated methods. Their use in operations is strongly discouraged.

{% endnote %}

#|
|| **Method** | **Description** ||
|| **Tasks (item)** | ||
|| [task.item.add](./task-item/task-item-add.md) | Creates a new task ||
|| [task.item.delete](./task-item/task-item-delete.md) | Deletes a task ||
|| [task.item.getdata](./task-item/task-item-get-data.md) | Returns an array of task data ||
|| [task.item.getmanifest](./task-item/task-item-get-manifest.md) | Returns a list of `task.item.*` methods with their descriptions ||
|| [task.item.list](./task-item/task-item-list.md) | Returns a list of tasks ||
|| [task.item.update](./task-item/task-item-update.md) | Updates task data ||
|| [task.item.getdescription](./task-item/task-item-get-description.md) | Returns the task description ||
|| [task.item.getfiles](./task-item/task-item-get-files.md) | Returns an array of links to files attached to the task ||
|| [task.item.getdependson](./task-item/task-item-get-dependson.md) | Returns an array of task IDs that the task depends on ||
|| [task.item.getallowedactions](./task-item/task-item-get-allowed-actions.md) | Returns an array of IDs of allowed actions on the task ||
|| [task.item.getallowedtaskactionsasstrings](./task-item/task-item-get-allowed-task-actions-as-strings.md) | Returns a list of allowed actions on the task ||
|| [task.item.isactionallowed](./task-item/task-item-is-action-allowed.md) | Checks if the action is allowed ||
|| [task.item.delegate](./task-item/task-item-delegate.md) | Delegates the task to a new user ||
|| [task.item.startexecution](./task-item/task-item-start-execution.md) | Changes the task status to "in progress" ||
|| [task.item.defer](./task-item/task-item-defer.md) | Changes the task status to "deferred" ||
|| [task.item.complete](./task-item/task-item-complete.md) | Changes the task status to "completed" or "conditionally completed (awaiting executor control)" ||
|| [task.item.renew](./task-item/task-item-renew.md) | Changes the task status to "not executed" ||
|| [task.item.approve](./task-item/task-item-approve.md) | Changes a task awaiting control to "completed" ||
|| [task.item.disapprove](./task-item/task-item-disapprove.md) | Changes a task awaiting control to "not executed" ||
|| [task.item.addtofavourite](./task-item/task-item-add-to-favourite.md) | Adds the task to Favorites ||
|| [task.item.deletefromfavorite](./task-item/task-item-delete-from-favorite.md) | Removes the task from Favorites ||
|| [task.item.addfile](./task-item/task-item-add-file.md) | Uploads a file to the task ||
|| [task.item.deletefile](./task-item/task-item-delete-file.md) | Removes the file attachment from the task ||
|| [task.logitem.list](./task-item/task-log-item-list.md) | Returns the history of task changes ||
|| **items** | ||
|| [task.items.getlist](./task-items-get-list.md) | Returns an array of tasks, each containing an array of fields ||
|| **comment** | ||
|| [task.comment.add](./task-comment-add.md) | Adds a comment to the task ||
|#