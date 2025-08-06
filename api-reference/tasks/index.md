# Tasks: Overview of Methods

Tasks in Bitrix24 are a unified space that helps organize team work: assigning small tasks and managing large projects. In tasks, you can track the progress of employees, control deadlines, and distribute responsibilities.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [Bitrix24 tasks](https://helpdesk.bitrix24.com/open/18034564/) 

## Features of Task Methods

In task methods, it is important to follow the order of parameters in the request as specified in the parameter tables. Otherwise, the request will execute with errors.

## Task Card

The task card can be divided into blocks:

- description
- system and custom fields
- comments, history, and time tracking

The task description contains information about what needs to be done. You can add checklists, files, and links to other tasks to the text.

Checklists help create a list of steps to complete the task. You can manage checklists using the group of methods [task.checklistitem.*](./checklist-item/index.md).

Create links to tasks using the method [task.dependence.add](./task-dependence-add.md). Delete them using the method [task.dependence.delete](./task-dependence-delete.md).

When creating a task, fill in the system fields: specify the assignee, watchers, deadline, tags, and so on.

If the system fields are not enough, you can create your own custom fields. They allow you to store information in various data formats: string, number, date with time, and yes/no. You can create, modify, retrieve, or delete custom task fields using the group of methods [task.item.userfield.*](./user-field/index.md).

In comments, discuss the task and write about the results of the work. Use the group of methods [task.commentitem.*](./comment-item/index.md) to work with comments.

The outcome of the work on the task can be recorded in a comment and fixed as a result. Manage task results using the group of methods [tasks.task.result.*](./result/index.md).

Time tracking in tasks monitors the time spent by an employee on the task. You can work with time tracking records using the group of methods [task.elapseditem.*](./elapsed-item/index.md).

All actions with the task are recorded and saved in the task history. To retrieve the history, use the method [tasks.task.history.list](./tasks-task-history-list.md).

{% note tip "User Documentation" %}

  - [How to create a task](https://helpdesk.bitrix24.com/open/18025566/)
  - [Checklists in tasks](https://helpdesk.bitrix24.com/open/17740572/)
  - [Time tracking in tasks](https://helpdesk.bitrix24.com/open/18009084/)
  - [Additional task features](https://helpdesk.bitrix24.com/open/17876766/)

{% endnote %}

## Linking Tasks with Other Objects

**Parent Task.** A task can have subtasks. In this case, it is considered a parent task. You can add a link to the parent task in the `PARENT_ID` parameter. You can get the task ID using the method [creating a task](./tasks-task-add.md) or [getting a list of tasks](./tasks-task-list.md).

**Group or Project.** A task is linked by the group ID `GROUP_ID`. You can get the ID using the method [creating a new group](../sonet-group/sonet-group-create.md) or the method [getting a list of groups](../sonet-group/socialnetwork-api-workgroup-list.md).

**User.** A task is linked to users by numeric IDs in the fields:
  - `CREATED_BY` — creator
  - `RESPONSIBLE_ID` — assignee
  - `ACCOMPLICES` — participants
  - `AUDITORS` — watchers
  - `CHANGED_BY`  — last user who modified the task
  - `STATUS_CHANGED_BY` — last user who changed the task status 
  - `CLOSED_BY` — user who completed the task

You can get the user ID using the method [user.get](../user/user-get.md).

**CRM.** You can link CRM objects to the task: contacts, companies, leads, deals, invoices, and SPAs. To link an object, specify its [identifier with prefix](../crm/data-types.md#object_type) in the `UF_CRM_TASK` parameter. For example, `C_3` for a contact with `id = 3`. You can get the identifier using the method [creating a new CRM item](../crm/universal/crm-item-add.md) or the method [getting a list of items](../crm/universal/crm-item-list.md).

**Email.** A task can be linked to an e-mail by identifier through the `UF_MAIL_MESSAGE` parameter.

{% note tip "User Documentation" %}

  - [How to create a subtask](https://helpdesk.bitrix24.com/open/17781634/)
  - [How to create a group and project](https://helpdesk.bitrix24.com/open/22796428/)

{% endnote %}

## Drive Files

You can attach Drive files to the task description. In the `UF_TASK_WEBDAV_FILES` parameter, pass an array with the identifiers of the Drive files. Precede each identifier with the prefix `n`, for example: `"UF_TASK_WEBDAV_FILES": ["n428", "n345"]`. You can obtain file identifiers in two ways.

Use one of the file upload methods:
  - [disk.storage.uploadfile](../disk/storage/disk-storage-upload-file.md)
  - [disk.folder.uploadfile](../disk/folder/disk-folder-upload-file.md)

Use one of the methods to get the list of files:
  - [disk.storage.getchildren](../disk/storage/disk-storage-get-children.md)
  - [disk.folder.getchildren ](../disk/folder/disk-folder-get-children.md)

Attach files to the task using the method [tasks.task.files.attach](./tasks-task-files-attach.md) if the task has already been created.

{% note tip "Typical use-cases and scenarios" %}

- [{#T}](../../tutorials/tasks/how-to-create-task-with-file.md)
- [{#T}](../../tutorials/tasks/how-to-upload-file-to-task.md)

{% endnote %}

## Flows

Flows are a tool that automates the distribution and execution of tasks. Employees do not need to search for who will do the task. They place tasks in the department flow, and it automatically assigns an assignee.

You can manage Flows using the group of methods [tasks.flow.Flow.*](./flow/index.md). 

{% note tip "User Documentation" %}

  - [Bitrix24 Flows: Getting Started](https://helpdesk.bitrix24.com/open/21415178/)

{% endnote %}

## Tasks in Scrum

Tasks in Scrum are standard Bitrix24 tasks with enhanced capabilities for working according to the Scrum methodology. In Scrum, the team can:

  - estimate task complexity using story points
  - attach tasks to epics
  - place tasks in the backlog and sprints
  - move tasks through sprint stages during work

Learn more about Scrum and its methods in the article [Scrum: Overview of Methods](../sonet-group/scrum/index.md).

{% note tip "User Documentation" %}

  - [Bitrix24.Scrum](https://helpdesk.bitrix24.com/open/14786248/)

{% endnote %}

## Task Management Modes

Kanban is a tool that helps visually represent task management in the form of columns and cards. Columns are stages of work, and cards are tasks. Kanban is used for managing tasks in groups and projects.

"My Planner" is a mode for managing your tasks in the form of a kanban. Each employee will have their own stages in "My Planner".

You can manage kanban stages and "My Planner" using the group of methods [task.stages.*](./stages/index.md).

## Daily Planner

"The Daily Planner" is a list of tasks, activities, and meetings that you have scheduled for the workday. The method [task.planner.getlist](./planner/index.md) retrieves the list of tasks from "The Daily Planner".

## Widgets

You can embed an application into the task card. Thanks to the embedding, you can use the application without leaving the card. 

- [Tab in the task card](../widgets/task/view-tab.md) `TASK_VIEW_TAB`
- [Right panel of the task card](../widgets/task/view-sidebar.md) `TASK_VIEW_SIDEBAR`
- [Link at the top of the task card](../widgets/task/view-top-panel.md) `TASK_VIEW_TOP_PANEL`

You can embed an application in the task list:

- [Context menu item in the list](../widgets/task/index.md) `TASK_LIST_CONTEXT_MENU` 

In the task management modes kanban or "My Planner," there are two more special places for embedding:

- [Main dropdown menu item](../widgets/task/list-toolbar.md) `TASK_USER_LIST_TOOLBAR`, `TASK_GROUP_LIST_TOOLBAR`
- [Main dropdown menu item near robot settings](../widgets/task/robot-designer-toolbar.md) `TASK_ROBOT_DESIGNER_TOOLBAR`


## Overview of Methods and Events {#all-methods}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Main

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [tasks.task.add](./tasks-task-add.md) | Creates a task ||
    || [tasks.task.update](./tasks-task-update.md) | Updates a task ||
    || [tasks.task.get](./tasks-task-get.md) | Retrieves information about a task by `id` ||
    || [tasks.task.list](./tasks-task-list.md) | Retrieves a list of tasks ||
    || [tasks.task.files.attach](./tasks-task-files-attach.md) | Attaches files to a task ||
    || [tasks.task.delegate](./tasks-task-delegate.md) | Delegates tasks ||
    || [tasks.task.counters.get](./tasks-task-counters-get.md) | Retrieves user counters ||
    || [tasks.task.start](./tasks-task-start.md) | Changes the task status to "in progress" ||
    || [tasks.task.pause](./tasks-task-pause.md) | Stops task execution and changes status to "waiting for execution" ||
    || [tasks.task.defer](./tasks-task-defer.md) | Changes the task status to "deferred" ||
    || [tasks.task.complete](./tasks-task-complete.md) | Changes the task status to "completed" ||
    || [tasks.task.renew](./tasks-task-renew.md) | Renews a task after it has been completed ||
    || [tasks.task.approve](./tasks-task-approve.md) | Approves a task ||
    || [tasks.task.disapprove](./tasks-task-disapprove.md) | Rejects a task ||
    || [tasks.task.delete](./tasks-task-delete.md) | Deletes a task ||
    || [tasks.task.startwatch](./tasks-task-start-watch.md) | Allows watching a task ||
    || [tasks.task.stopwatch](./tasks-task-stop-watch.md) | Stops watching a task ||
    || [tasks.task.favorite.add](./tasks-task-favorite-add.md) | Adds tasks to favorites ||
    || [tasks.task.favorite.remove](./tasks-task-favorite-remove.md) | Removes tasks from favorites ||
    || [tasks.task.getFields](./tasks-task-get-fields.md) | Retrieves available fields ||
    || [tasks.task.getaccess](./tasks-task-get-access.md) | Checks access to a task ||
    || [tasks.task.history.list](./tasks-task-history-list.md) | Retrieves task history ||
    || [tasks.task.mute](./tasks-task-mute.md) | Enables "Mute" mode ||
    || [tasks.task.unmute](./tasks-task-unmute.md) | Disables "Mute" mode ||
    || [task.dependence.add](./task-dependence-add.md) | Creates a dependency of one task on another ||
    || [task.dependence.delete](./task-dependence-delete.md) | Deletes a dependency of one task on another ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [OnTaskAdd](./events-tasks/on-task-add.md) | When a task is added ||
    || [OnTaskUpdate](./events-tasks/on-task-update.md) | When a task is updated ||
    || [OnTaskDelete](./events-tasks/on-task-delete.md) | When a task is deleted ||
    |#

{% endlist %}

### Task Result

#|
|| **Method** | **Description** ||
|| [tasks.task.result.addFromComment](./result/tasks-task-result-add-from-comment.md) | Adds a comment to the result ||
|| [tasks.task.result.list](./result/tasks-task-result-list.md) | Retrieves a list of task results ||
|| [tasks.task.result.deleteFromComment](./result/tasks-task-result-delete-from-comment.md) | Deletes a comment from the task result ||
|#

### Checklists

#|
|| **Method** | **Description** ||
|| [task.checklistitem.add](./checklist-item/task-checklist-item-add.md) | Adds a new checklist item to a task ||
|| [task.checklistitem.update](./checklist-item/task-checklist-item-update.md) | Updates the data of a checklist item ||
|| [task.checklistitem.get](./checklist-item/task-checklist-item-get.md) | Retrieves a checklist item by its `id` ||
|| [task.checklistitem.getlist](./checklist-item/task-checklist-item-get-list.md) | Retrieves a list of checklist items in a task ||
|| [task.checklistitem.moveafteritem](./checklist-item/task-checklist-item-move-after-item.md) | Moves a checklist item in the list after the specified one ||
|| [task.checklistitem.complete](./checklist-item/task-checklist-item-complete.md) | Marks a checklist item as completed ||
|| [task.checklistitem.renew](./checklist-item/task-checklist-item-renew.md) | Marks a completed checklist item as active again ||
|| [task.checklistitem.delete](./checklist-item/task-checklist-item-delete.md) | Deletes a checklist item ||
|| [task.checklistitem.isactionallowed](./checklist-item/task-checklist-item-is-action-allowed.md) | Checks if the action is allowed for the checklist item ||
|| [task.checklistitem.getmanifest](./checklist-item/task-checklist-item-get-manifest.md) | Retrieves the list of methods and their descriptions ||
|#

### Comments

#|
|| **Method** | **Description** ||
|| [task.commentitem.add](./comment-item/task-comment-item-add.md) | Creates a new comment for a task ||
|| [task.commentitem.update](./comment-item/task-comment-item-update.md) | Updates the comment data ||
|| [task.commentitem.get](./comment-item/task-comment-item-get.md) | Retrieves a comment for a task ||
|| [task.commentitem.getlist](./comment-item/task-comment-item-get-list.md) | Retrieves a list of comments for a task ||
|| [task.commentitem.delete](./comment-item/task-comment-item-delete.md) | Deletes a comment ||
|#

### Time Tracking

#|
|| **Method** | **Description** ||
|| [task.elapseditem.add](./elapsed-item/task-elapsed-item-add.md) | Adds time spent to a task ||
|| [task.elapseditem.update](./elapsed-item/task-elapsed-item-update.md) | Updates the parameters of the time tracking record ||
|| [task.elapseditem.get](./elapsed-item/task-elapsed-item-get.md) | Retrieves a time tracking record by its identifier ||
|| [task.elapseditem.getlist](./elapsed-item/task-elapsed-item-get-list.md) | Retrieves a list of time tracking records for a task ||
|| [task.elapseditem.delete](./elapsed-item/task-elapsed-item-delete.md) | Deletes a time tracking record ||
|| [task.elapseditem.isactionallowed](./elapsed-item/task-elapsed-item-is-action-allowed.md) | Checks if the action is allowed ||
|| [task.elapseditem.getmanifest](./elapsed-item/task-elapsed-item-get-manifest.md) | Retrieves the list of methods and their descriptions ||
|#

### Custom Fields

#|
|| **Method** | **Description** ||
|| [task.item.userfield.add](./user-field/task-item-user-field-add.md) | Creates a new field ||
|| [task.item.userfield.update](./user-field/task-item-user-field-update.md) | Updates the parameters of the field ||
|| [task.item.userfield.get](./user-field/task-item-user-field-get.md) | Retrieves a field by its identifier ||
|| [task.item.userfield.getlist](./user-field/task-item-user-field-get-list.md) | Retrieves a list of fields ||
|| [task.item.userfield.delete](./user-field/task-item-user-field-delete.md) | Deletes a field ||
|| [task.item.userfield.gettypes](./user-field/task-item-user-field-get-types.md) | Retrieves all available data types ||
|| [task.item.userfield.getfields](./user-field/task-item-user-field-get-fields.md) | Retrieves all available fields of the custom field ||
|#

### Kanban and "My Planner" Stages

#|
|| **Method** | **Description** ||
|| [task.stages.add](./stages/task-stages-add.md) | Adds stages to kanban or "My Planner" ||
|| [task.stages.update](./stages/task-stages-update.md) | Updates stages of kanban or "My Planner" ||
|| [task.stages.get](./stages/task-stages-get.md) | Retrieves stages of kanban or "My Planner" ||
|| [task.stages.canmovetask](./stages/task-stages-can-move-task.md) | Determines if the current user can move tasks in the specified object ||
|| [task.stages.movetask](./stages/task-stages-move-task.md) | Moves tasks from one stage to another ||
|| [task.stages.delete](./stages/task-stages-delete.md) | Deletes stages of kanban or "My Planner" ||
|#

### Daily Planner

#|
|| **Method** | **Description** ||
|| [task.planer.getList](./planner/task-planner-get-list.md) | Retrieves a list of tasks from "The Daily Planner" ||
|#

### Flows

#|
|| **Method** | **Description** ||
|| [tasks.flow.Flow.create](./flow/tasks-flow-flow-create.md) | Creates a flow ||
|| [tasks.flow.Flow.get](./flow/tasks-flow-flow-get.md) | Retrieves a flow ||
|| [tasks.flow.Flow.update](./flow/tasks-flow-flow-update.md) | Modifies a flow ||
|| [tasks.flow.Flow.delete](./flow/tasks-flow-flow-delete.md) | Deletes a flow ||
|| [tasks.flow.Flow.isExists](./flow/tasks-flow-flow-is-exists.md) | Checks if a flow with that name exists ||
|| [tasks.flow.Flow.activate](./flow/tasks-flow-flow-activate.md) | Turns a flow on or off ||
|#