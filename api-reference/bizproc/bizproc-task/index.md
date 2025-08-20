# Workflow Tasks: Overview of Methods

Workflows can create tasks for participants to gather additional information from them. For example, a task can be used to approve a document in accounting, authorize a vacation with a manager, or request a contract from a lawyer. Users can see their tasks in Bitrix24 and receive notifications.

There are four types of tasks:

- Document approval
- Document acknowledgment
- Request for additional information
- Request for additional information with rejection

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [Workflow Tasks](https://helpdesk.bitrix24.com/open/11466058/)

## Complete Task

You can complete a workflow task by its identifier using the [bizproc.task.complete](./bizproc-task-complete.md) method. To obtain the task identifier `TASK_ID`, use the [bizproc.task.list](./bizproc-task-list.md) method. You can only complete your own tasks.

Tasks that request additional information contain fields that the user must fill out. To complete such a task, pass the field values in the `FIELDS` object in the format `{"field_1": "value_1", ... , "field_N": "value_N"}`

-  `field_N` — symbolic identifier of the task field
-  `value_N` — field value

You can find out which fields need to be filled out from the response of the [bizproc.task.list](./bizproc-task-list.md) method. The object `"PARAMETERS": "Fields"` contains descriptions of all task fields.

## Delegate Task

A task can be delegated to another employee using the [bizproc.task.delegate](./bizproc-task-delegate.md) method. To do this, specify the task identifier `TASK_IDS` and the user identifiers:
- current assignee `FROM_USER_ID`
- new assignee `TO_USER_ID`

You can obtain the task identifier using the [bizproc.task.list](./bizproc-task-list.md) method, and the user identifier using the [user.get](../../user/user-get.md) method.

## Overview of Methods {#all-methods}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [bizproc.task.list](./bizproc-task-list.md) | Retrieves a list of workflow tasks ||
|| [bizproc.task.complete](./bizproc-task-complete.md) | Completes a workflow task ||
|| [bizproc.task.delegate](./bizproc-task-delegate.md) | Delegates a workflow task ||
|#