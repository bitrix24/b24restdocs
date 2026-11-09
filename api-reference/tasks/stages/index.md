# Stages of Kanban and "My Planner": Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Kanban displays the tasks of a group or project as columns and cards: a column is a stage of work, a card is a task.

"My Planner" is an employee's own Kanban of their tasks. Each employee has their own stages in it.

The methods in this section retrieve, add, update, and delete Kanban and "My Planner" stages and move tasks between stages.

> Quick navigation: [all methods](#all-methods)
>
> User documentation:
> - [Kanban: Track tasks in projects effectively](https://helpdesk.bitrix24.com/open/25762719/)
> - [Planner view in Tasks](https://helpdesk.bitrix24.com/open/18891600/)

## How to Get Started

1. Retrieve the stages with the [task.stages.get](./task-stages-get.md) method. To retrieve the Kanban stages of a group, pass the group ID in `entityId`; to retrieve the stages of your own "My Planner", pass `0`.
2. If the stage you need does not exist, add it with the [task.stages.add](./task-stages-add.md) method. The position is set by `AFTER_ID` — the ID of the stage after which the new one is added. Without `AFTER_ID` or with `0`, the new stage becomes the first one. The method returns the ID of the new stage.
3. Move the task to the stage with the [task.stages.movetask](./task-stages-move-task.md) method. To check in advance whether the user can move tasks in the group, use the [task.stages.canmovetask](./task-stages-can-move-task.md) method.
4. Check the result: retrieve the tasks of the stage with the [tasks.task.list](../tasks-task-list.md) method filtered by `STAGE_ID`, as described below in "How to Get the List of Tasks in a Stage".

## Connection of Kanban Stages and "My Planner" with Other Objects

**Workgroup.** A Kanban stage belongs to a group: the group ID is passed in `ENTITY_ID`. It is returned by the [create new group method](../../sonet-group/sonet-group-create.md) and the [get list of groups method](../../sonet-group/socialnetwork-api-workgroup-list.md).

**User.** The stage of "My Planner" is tied to the user. Any user, including the Bitrix24 administrator, can change stages only in their own planner.

{% note tip "User Documentation" %}

- [How to create a group and project](https://helpdesk.bitrix24.com/open/22796428/)

{% endnote %}

## How to Move Tasks Between Stages

You can move a task to another stage using the [task.stages.movetask](./task-stages-move-task.md) method. Within the stage, the task can be placed before or after another task — pass the `ID` of the neighboring task in `before` or `after`. Task IDs are returned by the [create task method](../tasks-task-add.md) and the [get list of tasks method](../tasks-task-list.md).

You can move a task:
- in the group's Kanban, if the user has permission to sort tasks in the group
- in "My Planner"

To check in advance whether the user can move tasks in the group, use the [task.stages.canmovetask](./task-stages-can-move-task.md) method.

## How to Get the List of Tasks in a Stage

You can retrieve the list of tasks in a stage using the [tasks.task.list](../tasks-task-list.md) method.

**Selection from the group's Kanban stages.** In the filter, pass `STAGE_ID` with the stage ID and `GROUP_ID` with the group ID. For the first stage, pass the array `[0, ID]` in `STAGE_ID`: tasks that have not yet been moved between stages are stored with stage `0` and appear in the first stage.

**Selection from "My Planner."** In the filter, pass `STAGE_ID` with the stage ID. The `[0, ID]` array is not needed here.

## Overview of Methods {#all-methods}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - work with stages of their own "My Planner" — any user
> - retrieve Kanban stages of a group — a user with permission to view the group's tasks
> - add, update, and delete Kanban stages of a group — the group owner or moderator
> - move tasks in a group's Kanban — a user with permission to sort tasks in the group

#|
|| **Method** | **Description** ||
|| [task.stages.add](./task-stages-add.md) | Adds Kanban or "My Planner" stages ||
|| [task.stages.update](./task-stages-update.md) | Updates Kanban or "My Planner" stages ||
|| [task.stages.get](./task-stages-get.md) | Retrieves Kanban or "My Planner" stages ||
|| [task.stages.delete](./task-stages-delete.md) | Deletes Kanban or "My Planner" stages ||
|| [task.stages.canmovetask](./task-stages-can-move-task.md) | Determines if the current user can move tasks in the specified object ||
|| [task.stages.movetask](./task-stages-move-task.md) | Moves tasks from one stage to another ||
|#
