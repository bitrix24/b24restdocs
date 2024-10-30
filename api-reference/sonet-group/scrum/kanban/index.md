# Kanban in Scrum: Overview of Methods

Kanban is a tool that helps visually represent task management in the form of columns and cards. Columns represent stages of work, while cards represent tasks. In the Scrum Kanban, the team can see all the tasks of the sprint and move them through the stages during the workflow.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [How to work in Scrum](https://helpdesk.bitrix24.com/open/21300770/)

## Connection of Kanban Stages with Other Objects

**Sprint.** The Kanban stage is linked to the sprint by the sprint identifier `sprintId`. You can obtain the identifier using the [add new sprint method](../sprint/tasks-api-scrum-sprint-add.md) or the [get list of sprints method](../sprint/tasks-api-scrum-sprint-list.md).

{% note tip "User Documentation" %}

- [Bitrix24.Scrum](https://helpdesk.bitrix24.com/open/14786248/)

{% endnote %}

## Features

The Scrum Kanban must include stages with the type new `NEW` and final `FINISH`.

Use the method for creating a Kanban stage only for active sprints, meaning those with the field `"status": "active"`.

## Tasks in Kanban

Tasks in the Kanban stage can be added using the [tasks.api.scrum.kanban.addTask](./tasks-api-scrum-kanban-add-task.md) method. This requires the identifiers of three objects:
  - sprint identifier `sprintId`. This can be obtained using the [get list of sprints method](../sprint/tasks-api-scrum-sprint-list.md)
  - task identifier `taskId`. This can be obtained using the [create task method](../../../tasks/tasks-task-add.md) or the [get list of tasks method](../../../tasks/tasks-task-list.md)
  - Kanban stage identifier `stageId`. This can be obtained using the [get Kanban stages method](./tasks-api-scrum-kanban-get-stages.md)

To remove a task from the Kanban, use the [tasks.api.scrum.kanban.deleteTask](./tasks-api-scrum-kanban-delete-task.md) method, specifying the sprint identifier `sprintId` and the task identifier `taskId`. The task will remain in the sprint on the planning page. This method will not move the task to the [backlog](../backlog/index.md).

## Overview of Methods {#all-methods}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [tasks.api.scrum.kanban.addStage](./tasks-api-scrum-kanban-add-stage.md) | Creates a Scrum Kanban stage ||
|| [tasks.api.scrum.kanban.updateStage](./tasks-api-scrum-kanban-update-stage.md) | Updates a Scrum Kanban stage ||
|| [tasks.api.scrum.kanban.getStages](./tasks-api-scrum-kanban-get-stages.md) | Retrieves Kanban stages by sprint `id` ||
|| [tasks.api.scrum.kanban.deleteStage](./tasks-api-scrum-kanban-delete-stage.md) | Deletes a stage ||
|| [tasks.api.scrum.kanban.addTask](./tasks-api-scrum-kanban-add-task.md) | Adds a task to the Scrum Kanban ||
|| [tasks.api.scrum.kanban.deleteTask](./tasks-api-scrum-kanban-delete-task.md) | Removes a task from the Scrum Kanban ||
|| [tasks.api.scrum.kanban.getFields](./tasks-api-scrum-kanban-get-fields.md) | Retrieves available fields of the Kanban stage ||
|#