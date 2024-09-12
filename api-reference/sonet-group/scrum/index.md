# Scrum

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can perform the method: depending on the method

Scrum is technically a project in *Bitrix24*. The "Scrum ID" refers to the `id` of the project/group and is passed in parameters/fields as `groupId`.

To create a Scrum, you can use the method for social network workgroups [sonet_group.create](../sonet-group-create.md), filling in the `SCRUM_MASTER_ID` field.

REST methods for working with Scrum:

#|
|| **Method** | **Description** ||
|| **Backlog** | ||
|| [tasks.api.scrum.backlog.add](backlog/tasks-api-scrum-backlog-add.md) | This method adds a backlog to Scrum. ||
|| [tasks.api.scrum.backlog.delete](backlog/tasks-api-scrum-backlog-delete.md) | This method deletes a backlog. ||
|| [tasks.api.scrum.backlog.get](backlog/tasks-api-scrum-backlog-get.md) | This method returns the field values of the backlog by Scrum ID. ||
|| [tasks.api.scrum.backlog.getFields](backlog/tasks-api-scrum-backlog-get-fields.md) | This method returns the available fields of the backlog. ||
|| [tasks.api.scrum.backlog.update](backlog/tasks-api-scrum-backlog-update.md) | This method modifies the backlog. ||
|| **Kanban** | ||
|| [tasks.api.scrum.kanban.addStage](kanban/tasks-api-scrum-kanban-add-stage.md) | This method creates a Kanban stage for Scrum. ||
|| [tasks.api.scrum.kanban.addTask](kanban/tasks-api-scrum-kanban-add-task.md) | This method adds a task to the Scrum Kanban. ||
|| [tasks.api.scrum.kanban.deleteStage](kanban/tasks-api-scrum-kanban-delete-stage.md) | This method deletes a stage. ||
|| [tasks.api.scrum.kanban.deleteTask](kanban/tasks-api-scrum-kanban-delete-task.md) | This method removes a task from the Scrum Kanban. ||
|| [tasks.api.scrum.kanban.getFields](kanban/tasks-api-scrum-kanban-get-fields.md) | This method returns the available fields of the Kanban stage. ||
|| [tasks.api.scrum.kanban.getStages](kanban/tasks-api-scrum-kanban-get-stages.md) | This method returns the stages of the Kanban by sprint ID. ||
|| [tasks.api.scrum.kanban.updateStage](kanban/tasks-api-scrum-kanban-update-stage.md) | This method modifies the Scrum Kanban stage. ||
|| **Epics** | ||
|| [tasks.api.scrum.epic.add](epic/tasks-api-scrum-epic-add.md) | This method adds an epic to Scrum. ||
|| [tasks.api.scrum.epic.delete](epic/tasks-api-scrum-epic-delete.md) | This method deletes an epic. ||
|| [tasks.api.scrum.epic.get](epic/tasks-api-scrum-epic-get.md) | This method returns the field values of the epic by its ID. ||
|| [tasks.api.scrum.epic.getFields](epic/tasks-api-scrum-epic-get-fields.md) | This method returns the available fields of the epic. ||
|| [tasks.api.scrum.epic.list](epic/tasks-api-scrum-epic-list.md) | This method returns a list of epics. ||
|| [tasks.api.scrum.epic.update](epic/tasks-api-scrum-epic-update.md) | This method modifies the epic in Scrum. ||
|| **Sprints** | ||
|| [tasks.api.scrum.sprint.add](sprint/tasks-api-scrum-sprint-add.md) | This method adds a sprint to Scrum. ||
|| [tasks.api.scrum.sprint.complete](sprint/tasks-api-scrum-sprint-complete.md) | This method completes the active sprint of the selected Scrum. ||
|| [tasks.api.scrum.sprint.delete](sprint/tasks-api-scrum-sprint-delete.md) | This method deletes a sprint. ||
|| [tasks.api.scrum.sprint.get](sprint/tasks-api-scrum-sprint-get.md) | This method returns the field values of the sprint by its ID. ||
|| [tasks.api.scrum.sprint.getFields](sprint/tasks-api-scrum-sprint-get-fields.md) | This method returns the available fields of the sprint. ||
|| [tasks.api.scrum.sprint.list](sprint/tasks-api-scrum-sprint-list.md) | This method returns a list of sprints. ||
|| [tasks.api.scrum.sprint.start](sprint/tasks-api-scrum-sprint-start.md) | This method starts the sprint. ||
|| [tasks.api.scrum.sprint.update](sprint/tasks-api-scrum-sprint-update.md) | This method modifies the sprint. ||
|| **Scrum Tasks** | ||
|| [tasks.api.scrum.task.get](task/tasks-api-scrum-task-get.md) | This method returns the field values of the Scrum task by its ID. ||
|| [tasks.api.scrum.task.getFields](task/tasks-api-scrum-task-get-fields.md) | This method returns the available fields of the Scrum task. ||
|| [tasks.api.scrum.task.update](task/tasks-api-scrum-task-update.md) | This method creates or modifies a Scrum task. ||
|#