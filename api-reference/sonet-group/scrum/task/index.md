# Scrum Tasks: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Scrum tasks are standard Bitrix24 tasks with enhanced capabilities for working within the Scrum methodology. In Scrum, the team can:

- assess task complexity using story points
- attach tasks to epics
- place tasks in the backlog and sprints
- move tasks through the stages of the sprint during the workflow

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Team collaboration with Scrum](https://helpdesk.bitrix24.com/open/21300770/)

## How to Add a Task in Scrum

A task can be created using the [tasks.task.add](../../../tasks/tasks-task-add.md) method or updated using the [tasks.task.update](../../../tasks/tasks-task-update.md) method. The task is linked to Scrum using the group identifier parameter `GROUP_ID`.

You can obtain the group identifier using the [create new group](../../sonet-group-create.md) method or the [get list of groups](../../socialnetwork-api-workgroup-list.md) method. A group is considered a Scrum group if the `SCRUM_MASTER_ID` field is filled.

## How to Get Started

1. Create a task using the [tasks.task.add](../../../tasks/tasks-task-add.md) method or retrieve an existing task using the [tasks.task.list](../../../tasks/tasks-task-list.md) method.
2. Retrieve the Scrum group identifier using the [socialnetwork.api.workgroup.list](../../socialnetwork-api-workgroup-list.md) method.
3. Link the task to Scrum and fill in Scrum fields using the [tasks.api.scrum.task.update](./tasks-api-scrum-task-update.md) method.
4. Check the Scrum task data using the [tasks.api.scrum.task.get](./tasks-api-scrum-task-get.md) method.

Once the task is linked to Scrum, you can use methods for managing Scrum tasks. For example, add story points and an epic using the [tasks.api.scrum.task.update](./tasks-api-scrum-task-update.md) method.

{% note tip "User Documentation" %}

- [Create a task](https://helpdesk.bitrix24.com/open/25865519/)
- [How to create a group and project](https://helpdesk.bitrix24.com/open/22796428/)

{% endnote %}

## Linking Scrum Tasks with Other Objects

**Backlog/Sprint.** A task is linked to the Scrum backlog or sprint through the identifier `entityId`. The backlog identifier can be obtained using the [get backlog fields by Scrum identifier](../backlog/tasks-api-scrum-backlog-get.md) method. The sprint identifier can be obtained using the [add new sprint](../sprint/tasks-api-scrum-sprint-add.md) method or the [get list of sprints](../sprint/tasks-api-scrum-sprint-list.md) method. 

**Epic.** A task is attached to an epic by the identifier `epicId`. The epic identifier can be obtained using the [add epic to Scrum](../epic/tasks-api-scrum-epic-add.md) method or the [get list of epics](../epic/tasks-api-scrum-epic-list.md) method. 

## Overview of Methods {#all-methods}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#|
|| **Method** | **Description** ||
|| [tasks.api.scrum.task.update](./tasks-api-scrum-task-update.md) | Creates or updates a Scrum task ||
|| [tasks.api.scrum.task.get](./tasks-api-scrum-task-get.md) | Retrieves field values of a Scrum task by `id` ||
|| [tasks.api.scrum.task.getFields](./tasks-api-scrum-task-get-fields.md) | Retrieves available fields of a Scrum task ||
|#
