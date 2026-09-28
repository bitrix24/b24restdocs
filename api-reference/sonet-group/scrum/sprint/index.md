# Sprints in Scrum: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A sprint is a short iterative Scrum cycle during which a team completes a set of tasks. The `tasks.api.scrum.sprint.*` methods create, update, start, complete, and delete sprints and return their data.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Team collaboration with Scrum](https://helpdesk.bitrix24.com/open/21300770/)

## Connection of Sprints with Other Objects

**Workgroup.** Sprints are linked to a workgroup (Scrum) by the group identifier `groupId`. You can obtain the identifier using the [create new group](../../sonet-group-create.md) method or the [get list of groups](../../socialnetwork-api-workgroup-list.md) method. A group is considered a Scrum if the `SCRUM_MASTER_ID` field is filled.

**Task.** A task belongs to a sprint when its `entityId` field contains the sprint identifier. You can change the field using the [tasks.api.scrum.task.update](../task/tasks-api-scrum-task-update.md) method.

**Backlog.** When a sprint is completed, its unfinished tasks move to the Scrum [backlog](../backlog/index.md). When a sprint is deleted, all of its tasks move there.

## How to Get Started

1. Retrieve the group identifier using the [socialnetwork.api.workgroup.list](../../socialnetwork-api-workgroup-list.md) method.
2. Create a sprint with the `planned` status using the [tasks.api.scrum.sprint.add](./tasks-api-scrum-sprint-add.md) method.
3. Add tasks to a sprint using the [tasks.api.scrum.task.update](../task/tasks-api-scrum-task-update.md) method.
4. Start a sprint using the [tasks.api.scrum.sprint.start](./tasks-api-scrum-sprint-start.md) method.
5. Complete the active sprint using the [tasks.api.scrum.sprint.complete](./tasks-api-scrum-sprint-complete.md) method.

## Sprint Lifecycle

A sprint goes through the statuses `planned` → `active` → `completed`. A Scrum can have only one active sprint: complete the current one before starting the next.

- [tasks.api.scrum.sprint.start](./tasks-api-scrum-sprint-start.md) starts a planned sprint by the sprint identifier
- [tasks.api.scrum.sprint.complete](./tasks-api-scrum-sprint-complete.md) completes the active sprint by the group identifier, not the sprint identifier
- [tasks.api.scrum.sprint.delete](./tasks-api-scrum-sprint-delete.md) deletes a sprint in any status

## Overview of Methods {#all-methods}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the methods: `tasks.api.scrum.sprint.list` and `tasks.api.scrum.sprint.getFields` — any user; `tasks.api.scrum.sprint.start` and `tasks.api.scrum.sprint.complete` — the Scrum owner or moderator, or a Bitrix24 administrator; other methods — any user with access to the Scrum

#|
|| **Method** | **Description** ||
|| [tasks.api.scrum.sprint.add](./tasks-api-scrum-sprint-add.md) | Adds a sprint to Scrum ||
|| [tasks.api.scrum.sprint.update](./tasks-api-scrum-sprint-update.md) | Updates a sprint ||
|| [tasks.api.scrum.sprint.get](./tasks-api-scrum-sprint-get.md) | Retrieves a sprint by its identifier ||
|| [tasks.api.scrum.sprint.list](./tasks-api-scrum-sprint-list.md) | Retrieves a list of sprints ||
|| [tasks.api.scrum.sprint.delete](./tasks-api-scrum-sprint-delete.md) | Deletes a sprint ||
|| [tasks.api.scrum.sprint.start](./tasks-api-scrum-sprint-start.md) | Starts a sprint ||
|| [tasks.api.scrum.sprint.complete](./tasks-api-scrum-sprint-complete.md) | Completes an active sprint of the selected Scrum ||
|| [tasks.api.scrum.sprint.getFields](./tasks-api-scrum-sprint-get-fields.md) | Retrieves available fields of a sprint ||
|#
