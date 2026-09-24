# Kanban in Scrum: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Kanban is a tool that helps visually represent task management in the form of columns and cards. Columns represent stages of work, while cards represent tasks. In the Scrum Kanban, the team can see all the tasks of the sprint and move them through the stages during the workflow.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Team collaboration with Scrum](https://helpdesk.bitrix24.com/open/21300770/)

## Connection of Kanban Stages with Other Objects

**Sprint.** The Kanban stage is linked to the sprint by the sprint identifier `sprintId`. You can obtain the identifier using the [add new sprint method](../sprint/tasks-api-scrum-sprint-add.md) or the [get list of sprints method](../sprint/tasks-api-scrum-sprint-list.md).

**Task.** A task is linked to a stage by the `taskId`, `stageId`, and `sprintId` identifiers. The task and sprint must belong to the same Scrum group.

## Kanban Stage Data

The [tasks.api.scrum.kanban.getStages](./tasks-api-scrum-kanban-get-stages.md) method returns an array of stages sorted by the `sort` field.

#|
|| **Field** | **Description** | **Example** ||
|| `id` | Stage identifier | `58` ||
|| `name` | Stage name | `New` ||
|| `sort` | Sort order | `100` ||
|| `type` | Stage type: `NEW`, `WORK`, or `FINISH` | `NEW` ||
|| `sprintId` | Sprint identifier | `5` ||
|| `color` | Stage color as a six-character HEX code without `#` | `00C4FB` ||
|#

```json
{
    "result": [
        {
            "id": "58",
            "name": "New",
            "sort": "100",
            "type": "NEW",
            "sprintId": "5",
            "color": "00C4FB"
        }
    ]
}
```

## How to Get Started

1. Retrieve the active sprint identifier using the [tasks.api.scrum.sprint.list](../sprint/tasks-api-scrum-sprint-list.md) method.
2. Check the current stage configuration using the [tasks.api.scrum.kanban.getStages](./tasks-api-scrum-kanban-get-stages.md) method.
3. Create or update stages using the [tasks.api.scrum.kanban.addStage](./tasks-api-scrum-kanban-add-stage.md) and [tasks.api.scrum.kanban.updateStage](./tasks-api-scrum-kanban-update-stage.md) methods.
4. Add a task to a sprint stage using the [tasks.api.scrum.kanban.addTask](./tasks-api-scrum-kanban-add-task.md) method.

{% note tip "User Documentation" %}

- [Bitrix24 Scrum: Getting started](https://helpdesk.bitrix24.com/open/25787711/)

{% endnote %}

## Features

Methods accept three stage types: new `NEW`, work `WORK`, and final `FINISH`. If `type` is omitted when creating a stage, the method sets it to `WORK`. The default values for `sort` and `color` are `100` and `00C4FB`. There is no fixed list of values for `color`: pass a six-character HEX code without `#`.

## Method Access

[Retrieving stages](./tasks-api-scrum-kanban-get-stages.md) requires permission to view tasks in the Scrum group. Creating, updating, and deleting stages, as well as adding and removing tasks, requires permission to edit tasks in the group. The [tasks.api.scrum.kanban.getFields](./tasks-api-scrum-kanban-get-fields.md) method returns the field reference without checking permissions for a specific group.

## Tasks in Kanban

Tasks in the Kanban stage can be added using the [tasks.api.scrum.kanban.addTask](./tasks-api-scrum-kanban-add-task.md) method. This requires the identifiers of three objects:

- sprint identifier `sprintId`. This can be obtained using the [get list of sprints method](../sprint/tasks-api-scrum-sprint-list.md)
- task identifier `taskId`. This can be obtained using the [create task method](../../../tasks/tasks-task-add.md) or the [get list of tasks method](../../../tasks/tasks-task-list.md)
- Kanban stage identifier `stageId`. This can be obtained using the [get Kanban stages method](./tasks-api-scrum-kanban-get-stages.md)

To remove a task from the Kanban, use the [tasks.api.scrum.kanban.deleteTask](./tasks-api-scrum-kanban-delete-task.md) method, specifying the sprint identifier `sprintId` and the task identifier `taskId`. The task will remain in the sprint on the planning page. This method will not move the task to the [backlog](../backlog/index.md).

## Overview of Methods {#all-methods}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

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
