# Scrum: Overview of Methods

Scrum is a flexible way to organize work with tasks. The team can break down a large project and execute it gradually, one piece at a time.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [Bitrix24.Scrum](https://helpdesk.bitrix24.com/open/14786248/)

## Scrum in Bitrix24

Technically, Scrum is a group. The Scrum identifier `groupId` in the method parameters and fields of Scrum elements is the `id` of the group.

To create a Scrum, use the [create new group method](../sonet-group-create.md). A group is considered a Scrum if the `SCRUM_MASTER_ID` field is filled.

{% note tip "User documentation" %}

- [How to create a group and project](https://helpdesk.bitrix24.com/open/22796428/)

{% endnote %}

## Scrum Elements

Tasks in Scrum are standard Bitrix24 tasks with enhanced capabilities for working according to the Scrum methodology. To create or modify tasks, the group of methods [tasks.api.scrum.task.*](./task/index.md) is used.

The Scrum team gathers all project tasks in one place—the backlog. Tasks, wishes, ideas, and feedback are recorded and prioritized. Working with the backlog can be done using the group of methods [tasks.api.scrum.backlog.*](./backlog/index.md).

To make the backlog more visual, tasks are attached to epics. An epic is a theme, context, or larger goal to which a task relates. To create, modify, retrieve, or delete epics, the group of methods [tasks.api.scrum.epic.*](./epic/index.md) is used.

Team members review the backlog and decide which tasks to take on in the sprint. A sprint is a short iterative cycle during which the team completes specific work. Managing sprints can be done using the group of methods [tasks.api.scrum.sprint.*](./sprint/index.md).

The team moves tasks through the stages of the Kanban, working on the tasks of the sprint. Kanban is a tool that helps visually represent work with tasks in the form of columns and cards. Columns are the stages of work, and cards are the tasks. Working with stages and managing tasks in Kanban is performed using the methods [tasks.api.scrum.kanban.*](./kanban/index.md).

## Overview of Methods {#all-methods} 

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

### Backlog

#|
|| **Method** | **Description** ||
|| [tasks.api.scrum.backlog.add](./backlog/tasks-api-scrum-backlog-add.md) | Adds a backlog to Scrum ||
|| [tasks.api.scrum.backlog.update](./backlog/tasks-api-scrum-backlog-update.md) | Updates the backlog ||
|| [tasks.api.scrum.backlog.get](./backlog/tasks-api-scrum-backlog-get.md) | Retrieves field values of the backlog by Scrum identifier ||
|| [tasks.api.scrum.backlog.delete](./backlog/tasks-api-scrum-backlog-delete.md) | Deletes the backlog ||
|| [tasks.api.scrum.backlog.getFields](./backlog/tasks-api-scrum-backlog-get-fields.md) | Retrieves available fields of the backlog ||
|#

### Kanban

#|
|| **Method** | **Description** ||
|| [tasks.api.scrum.kanban.addStage](./kanban/tasks-api-scrum-kanban-add-stage.md) | Creates a Kanban stage for Scrum ||
|| [tasks.api.scrum.kanban.updateStage](./kanban/tasks-api-scrum-kanban-update-stage.md) | Updates a Kanban stage for Scrum ||
|| [tasks.api.scrum.kanban.getStages](./kanban/tasks-api-scrum-kanban-get-stages.md) | Retrieves Kanban stages by `id` of the sprint ||
|| [tasks.api.scrum.kanban.deleteStage](./kanban/tasks-api-scrum-kanban-delete-stage.md) | Deletes a stage ||
|| [tasks.api.scrum.kanban.addTask](./kanban/tasks-api-scrum-kanban-add-task.md) | Adds a task to the Scrum Kanban ||
|| [tasks.api.scrum.kanban.deleteTask](./kanban/tasks-api-scrum-kanban-delete-task.md) | Deletes a task from the Scrum Kanban ||
|| [tasks.api.scrum.kanban.getFields](./kanban/tasks-api-scrum-kanban-get-fields.md) | Retrieves available fields of the Kanban stage ||
|#

### Epics

#|
|| **Method** | **Description** ||
|| [tasks.api.scrum.epic.add](./epic/tasks-api-scrum-epic-add.md) | Adds an epic to Scrum ||
|| [tasks.api.scrum.epic.update](./epic/tasks-api-scrum-epic-update.md) | Updates an epic in Scrum ||
|| [tasks.api.scrum.epic.get](./epic/tasks-api-scrum-epic-get.md) | Retrieves field values of the epic by its `id` ||
|| [tasks.api.scrum.epic.list](./epic/tasks-api-scrum-epic-list.md) | Retrieves a list of epics ||
|| [tasks.api.scrum.epic.delete](./epic/tasks-api-scrum-epic-delete.md) | Deletes an epic ||
|| [tasks.api.scrum.epic.getFields](./epic/tasks-api-scrum-epic-get-fields.md) | Retrieves available fields of the epic ||
|#

### Sprints

#|
|| **Method** | **Description** ||
|| [tasks.api.scrum.sprint.add](./sprint/tasks-api-scrum-sprint-add.md) | Adds a sprint to Scrum ||
|| [tasks.api.scrum.sprint.update](./sprint/tasks-api-scrum-sprint-update.md) | Updates a sprint ||
|| [tasks.api.scrum.sprint.start](./sprint/tasks-api-scrum-sprint-start.md) | Starts a sprint ||
|| [tasks.api.scrum.sprint.complete](./sprint/tasks-api-scrum-sprint-complete.md) | Completes the active sprint of the selected Scrum ||
|| [tasks.api.scrum.sprint.get](./sprint/tasks-api-scrum-sprint-get.md) | Retrieves field values of the sprint by its `id` ||
|| [tasks.api.scrum.sprint.list](./sprint/tasks-api-scrum-sprint-list.md) | Retrieves a list of sprints ||
|| [tasks.api.scrum.sprint.delete](./sprint/tasks-api-scrum-sprint-delete.md) | Deletes a sprint ||
|| [tasks.api.scrum.sprint.getFields](./sprint/tasks-api-scrum-sprint-get-fields.md) | Retrieves available fields of the sprint ||
|#

### Scrum Tasks

#|
|| **Method** | **Description** ||
|| [tasks.api.scrum.task.update](./task/tasks-api-scrum-task-update.md) | Creates or updates a Scrum task ||
|| [tasks.api.scrum.task.get](./task/tasks-api-scrum-task-get.md) | Retrieves field values of the Scrum task by `id` ||
|| [tasks.api.scrum.task.getFields](./task/tasks-api-scrum-task-get-fields.md) | Retrieves available fields of the Scrum task ||
|#