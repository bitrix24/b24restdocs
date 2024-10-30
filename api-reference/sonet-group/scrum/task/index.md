# Scrum Tasks: Overview of Methods

Scrum tasks are standard Bitrix24 tasks with enhanced capabilities for working within the Scrum methodology. In Scrum, the team can:
- assess task complexity using story points
- attach tasks to epics
- place tasks in the backlog and sprints
- move tasks through the stages of the sprint during the workflow

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [How to work in Scrum](https://helpdesk.bitrix24.com/open/21300770/)

## How to Add a Task in Scrum

A task can be created using the [tasks.task.add](../../../tasks/tasks-task-add.md) method or updated using the [tasks.task.update](../../../tasks/tasks-task-update.md) method. The task's association with Scrum is specified in the group identifier parameter `GROUP_ID`. 

You can obtain the group identifier using the [create new group](../../sonet-group-create.md) method or the [get list of groups](../../socialnetwork-api-workgroup-list.md) method. A group is considered a Scrum group if the `SCRUM_MASTER_ID` field is filled.

Once the task is linked to Scrum, you can use methods for managing Scrum tasks. For example, add story points and an epic using the [tasks.api.scrum.task.update](./tasks-api-scrum-task-update.md) method.

{% note tip "User Documentation" %}

- [How to create a task](https://helpdesk.bitrix24.com/open/18025566/)
- [How to create a group and project](https://helpdesk.bitrix24.com/open/22796428/)

{% endnote %}

## Linking Scrum Tasks with Other Objects

**Backlog/Sprint.** A task is linked to the Scrum backlog or sprint through the identifier `entityId`. The backlog identifier can be obtained using the [get backlog fields by Scrum identifier](../backlog/tasks-api-scrum-backlog-get.md) method. The sprint identifier can be obtained using the [add new sprint](../sprint/tasks-api-scrum-sprint-add.md) method or the [get list of sprints](../sprint/tasks-api-scrum-sprint-list.md) method. 

**Epic.** A task is attached to an epic by the identifier `epicId`. The epic identifier can be obtained using the [add epic to Scrum](../epic/tasks-api-scrum-epic-add.md) method or the [get list of epics](../epic/tasks-api-scrum-epic-list.md) method. 

## Overview of Methods {#all-methods}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#|
|| **Method** | **Description** ||
|| [tasks.api.scrum.task.update](./tasks-api-scrum-task-update.md) | Creates or updates a Scrum task ||
|| [tasks.api.scrum.task.get](./tasks-api-scrum-task-get.md) | Retrieves field values of a Scrum task by `id` ||
|| [tasks.api.scrum.task.getFields](./tasks-api-scrum-task-get-fields.md) | Retrieves available fields of a Scrum task ||
|#