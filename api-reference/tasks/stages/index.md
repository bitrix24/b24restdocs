# Stages of Kanban and "My Planner": Overview of Methods

Kanban is a tool that helps visually represent task management through columns and cards. Columns represent stages of work, while cards represent tasks. Kanban is used for managing tasks in groups and projects.

"My Planner" is a task management mode presented in a Kanban format. Each employee will have their own stages in "My Planner."

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: 
>   - [Kanban: how to track tasks in projects](https://helpdesk.bitrix24.com/open/18113582/)
>   - [How to work with tasks in "My Planner" mode](https://helpdesk.bitrix24.com/open/18891600/)

## Connection of Kanban Stages and "My Planner" with Other Objects

**Workgroup.** The Kanban stage is linked to the workgroup by the object identifier `ENTITY_ID`. The identifier can be obtained using the [create new group method](../../sonet-group/sonet-group-create.md) or the [get list of groups method](../../sonet-group/socialnetwork-api-workgroup-list.md).

**User.** The stage of "My Planner" is tied to the user. Any user, including the account administrator, can change stages only in their own planner.

**Stage.** Kanban or "My Planner" stages can be interconnected. Specify the stage identifier in the `AFTER_ID` parameter, indicating after which stage the new stage should be added. The identifier can be obtained using the [add new stage method](task-stages-add.md) or the [get list of stages method](./task-stages-get.md). If the identifier is left blank or set to zero, the stage will be added to the beginning of the Kanban.

{% note tip "User Documentation" %}

- [How to create a group and project](https://helpdesk.bitrix24.com/open/22796428/)

{% endnote %}

## How to Move Tasks Between Stages

You can move a task from one stage to another using the [task.stages.movetask](./task-stages-move-task.md) method. This method allows you to specify the `ID` of the task before or after which the moving task should be placed. You can obtain the task identifier using the [create task method](../tasks-task-add.md) or the [get list of tasks method](../tasks-task-list.md).

You can move a task:
- in the group's Kanban if you have access to the group
- in "My Planner"

The method [task.stages.canmovetask](./task-stages-can-move-task.md) allows you to check if the task can be moved.

## How to Get the List of Tasks in a Stage

You can obtain the list of tasks in a stage using the [task.task.list](../tasks-task-list.md) method.

**Selection from the group's Kanban stages.** In the filter array, pass the key `STAGE_ID` with the `ID` of the desired stage. If you are selecting tasks from the first stage, pass an array of values in `STAGE_ID` consisting of `0` and the `ID` of your stage `[0, ID]`. Additionally, filter the selection by group.

**Selection from "My Planner."** In the filter array, pass the key `STAGE_ID` with the `ID` of the stage. There is no need to pass an array of values.

You can obtain the Kanban or "My Planner" stage identifier using the [add stage method](./task-stages-add.md) or the [get list of stages method](./task-stages-get.md).

## Overview of Methods {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user for "My Planner" stages
> - any user with access to the group for Kanban stages

#|
|| **Method** | **Description** ||
|| [task.stages.add](./task-stages-add.md) | Adds Kanban or "My Planner" stages ||
|| [task.stages.update](./task-stages-update.md) | Updates Kanban or "My Planner" stages ||
|| [task.stages.get](./task-stages-get.md) | Retrieves Kanban or "My Planner" stages ||
|| [task.stages.canmovetask](./task-stages-can-move-task.md) | Determines if the current user can move tasks in the specified object ||
|| [task.stages.movetask](./task-stages-move-task.md) | Moves tasks from one stage to another ||
|| [task.stages.delete](./task-stages-delete.md) | Deletes Kanban or "My Planner" stages ||
|#