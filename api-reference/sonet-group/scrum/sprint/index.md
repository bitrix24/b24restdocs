# Sprints in Scrum: Overview of Methods

A sprint is a short iterative cycle during which a team accomplishes specific work. This format allows for obtaining a small but predictable result within a clear timeframe.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [Bitrix24.Scrum](https://helpdesk.bitrix24.com/open/14786248/) 

## Connection of Sprints with Other Objects

**Workgroup.** Sprints are linked to a workgroup (scrum) by the group identifier `groupId`. You can obtain the identifier using the [create new group](../../sonet-group-create.md) method or the [get list of groups](../../socialnetwork-api-workgroup-list.md) method. A group is considered a scrum if the `SCRUM_MASTER_ID` field is filled.

{% note tip "User Documentation" %}

- [How to create a group and project](https://helpdesk.bitrix24.com/open/22796428/)

{% endnote %}

## How to Start a Sprint

The method [tasks.api.scrum.sprint.start](./tasks-api-scrum-sprint-start.md) starts a sprint by the sprint identifier `id`. You can obtain the sprint identifier using the [create new sprint](./tasks-api-scrum-sprint-add.md) method or the [get list of sprints](./tasks-api-scrum-sprint-list.md) method. You can only start a planned sprint, meaning one with the status `planned`. The status of a started sprint will change to `active`.

## How to Complete an Active Sprint

The method [tasks.api.scrum.sprint.complete](./tasks-api-scrum-sprint-complete.md) completes an active sprint by the group identifier `id`, not the sprint. You can obtain the identifier using the [create new group](../../sonet-group-create.md) method or the [get list of groups](../../socialnetwork-api-workgroup-list.md) method. The status of a completed sprint will change from `active` to `completed`.

## Overview of Methods {#all-methods}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#|
|| **Method** | **Description** ||
|| [tasks.api.scrum.sprint.add](./tasks-api-scrum-sprint-add.md) | Adds a sprint to Scrum ||
|| [tasks.api.scrum.sprint.update](./tasks-api-scrum-sprint-update.md) | Updates a sprint ||
|| [tasks.api.scrum.sprint.start](./tasks-api-scrum-sprint-start.md) | Starts a sprint ||
|| [tasks.api.scrum.sprint.complete](./tasks-api-scrum-sprint-complete.md) | Completes an active sprint of the selected Scrum ||
|| [tasks.api.scrum.sprint.get](./tasks-api-scrum-sprint-get.md) | Retrieves the field values of a sprint by its `id` ||
|| [tasks.api.scrum.sprint.list](./tasks-api-scrum-sprint-list.md) | Retrieves a list of sprints ||
|| [tasks.api.scrum.sprint.delete](./tasks-api-scrum-sprint-delete.md) | Deletes a sprint ||
|| [tasks.api.scrum.sprint.getFields](./tasks-api-scrum-sprint-get-fields.md) | Retrieves available fields of a sprint ||
|#