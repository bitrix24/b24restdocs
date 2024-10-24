# Time Tracking in Tasks: Overview of Methods

With the Time Tracking in Tasks tool, you can:
- monitor the time spent by an employee on specific tasks
- enhance the transparency of task execution
- analyze employee efficiency

> Quick Navigation: [all methods and events](#all-methods) 
> 
> User Documentation: [time tracking in tasks](https://helpdesk.bitrix24.com/open/18009084/) 

## Linking Time Spent with Other Objects

**Task.** A single task can have multiple entries for time spent. These entries are linked by the task identifier `TASKID`. You can obtain it using the [create new task](../tasks-task-add.md) method or the [get task list](../tasks-task-list.md) method.

**User.** The time spent entry is associated with a user by their identifier `USER_ID`. You can get the user identifier using the [user.get](../../user/user-get.md) method.

{% note tip "User Documentation" %}

- [Bitrix24 tasks](https://helpdesk.bitrix24.com/open/18034564/)

{% endnote %}

## Who Can Add or Modify an Entry

To add, modify, or delete a time spent entry, you need access permissions to the task. You can check permissions using the special method [task.elapseditem.isactionallowed](./task-elapsed-item-is-action-allowed.md).

## Reference Information on Methods

You can find up-to-date information about methods for working with time spent using the [task.elapseditem.getmanifest](./task-elapsed-item-get-manifest.md) method. It is recommended to use it only as a reference, as the structure of the method's response may change at any time by the developer.

## Overview of Methods {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#| 
|| **Method** | **Description** ||
|| [task.elapseditem.add](./task-elapsed-item-add.md) | Adds time spent to a task ||
|| [task.elapseditem.update](./task-elapsed-item-update.md) | Modifies the parameters of a time spent entry ||
|| [task.elapseditem.get](./task-elapsed-item-get.md) | Returns a time spent entry by its identifier ||
|| [task.elapseditem.getlist](./task-elapsed-item-get-list.md) | Returns a list of time spent entries for a task ||
|| [task.elapseditem.delete](./task-elapsed-item-delete.md) | Deletes a time spent entry ||
|| [task.elapseditem.isactionallowed](./task-elapsed-item-is-action-allowed.md) | Checks if the action is allowed ||
|| [task.elapseditem.getmanifest](./task-elapsed-item-get-manifest.md) | Returns a list of methods and their descriptions ||
|#