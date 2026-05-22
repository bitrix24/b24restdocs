# Tasks: method overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Tasks in Bitrix24 are a unified workspace that helps organize team workflows: assigning small assignments and managing large projects. With tasks, you can track employee progress, monitor deadlines, and distribute responsibilities.

> Quick links: [all methods and events](#all-methods) 
> 
> User documentation: [Bitrix24 tasks](https://helpdesk.bitrix24.com/open/18034564/) 

## Task method features

When using task methods, you must follow the parameter order specified in the parameter tables. Otherwise, the request will execute with errors.

## Task card

A task card can be divided into blocks:

- description
- system and user fields
- task chat
- history and time tracking

The task description contains information about what needs to be done. You can add checklists, files, and links to other tasks to the text.

Checklists help create a list of steps to complete a task. You can manage checklists using the [task.checklistitem.*](./checklist-item/index.md) method group.

If you need to avoid filling in the same fields manually for recurring tasks, use [task templates](./template/index.md). A template allows you to pre-save the title, description, participants, deadlines, project, checklist, and other parameters of a future task.

Create a link to tasks using the [task.dependence.add](./task-dependence-add.md) method. Delete them using the [task.dependence.delete](./task-dependence-delete.md) method.

When creating a task, fill in the system fields: specify the responsible person, observers, deadline, tags, and so on.

If system fields are not enough, you can create your own user fields. They allow you to store information in various data formats: string, number, date with time, and yes/no. You can create, change, retrieve, or delete task user fields using the [task.item.userfield.*](./user-field/index.md) method group.

In the new task card, discussions take place in the task chat. Since module version `tasks 25.700.0`, comments have been moved to the chat, so use task chat and messenger methods to work with messages. See details in the article [New task card: overview of changes](./tasks-new.md).

The result of working on a task can be written in a comment and recorded as a result. Manage task results using the [tasks.task.result.*](./result/index.md) method group.

Time tracking in tasks monitors the time spent by an employee on a task. You can work with time tracking records using the [task.elapseditem.*](./elapsed-item/index.md) method group.

All actions with a task are recorded and retained in the task history. To retrieve the history, use the [tasks.task.history.list](./tasks-task-history-list.md) method.

{% note tip "User documentation" %}

  - [How to create a task](https://helpdesk.bitrix24.com/open/18025566/)
  - [Checklists in tasks](https://helpdesk.bitrix24.com/open/17740572/)
  - [Time tracking in tasks](https://helpdesk.bitrix24.com/open/18009084/)
  - [Additional task features](https://helpdesk.bitrix24.com/open/17876766/)

{% endnote %}

## Connection with Other Objects

**Parent task.** A task can have subtasks. In this case, it is considered a parent task. You can add a link to a parent task in the `PARENT_ID` parameter. You can retrieve a task identifier using the [task creation](./tasks-task-add.md) method or the [task list retrieval](./tasks-task-list.md) method.

**Group or project.** A task is linked by the group identifier `GROUP_ID`. You can retrieve the identifier using the [new group creation](../sonet-group/sonet-group-create.md) method or the [group list retrieval](../sonet-group/socialnetwork-api-workgroup-list.md) method.**User.** A task is linked to users via numeric identifiers in the following fields:
  - `CREATED_BY` — Creator
  - `RESPONSIBLE_ID` — Responsible person
  - `ACCOMPLICES` — Co-executors
  - `AUDITORS` — Observers
  - `CHANGED_BY` — Last user who changed the task
  - `STATUS_CHANGED_BY` — Last user who changed the task status
  - `CLOSED_BY` — User who completed the task

You can retrieve a user identifier using the [user.get](../user/user-get.md) method.

**CRM.** You can link CRM objects to a task: contacts, companies, leads, deals, invoices, and SPAs. To link an object, specify its [identifier with a prefix](../crm/data-types.md#object_type) in the `UF_CRM_TASK` parameter. For example, `C_3` for contact with `id = 3`. You can retrieve the identifier using the [create new CRM item](../crm/universal/crm-item-add.md) method or the [retrieve item list](../crm/universal/crm-item-list.md) method.

**Webmail.** A task can be linked to an email by identifier via the `UF_MAIL_MESSAGE` parameter.

{% note tip "User documentation" %}

  - [How to create a subtask](https://helpdesk.bitrix24.com/open/17781634/)
  - [How to create a group and project](https://helpdesk.bitrix24.com/open/22796428/)

{% endnote %}

## Drive files

You can attach Drive files to a task description. In the `UF_TASK_WEBDAV_FILES` parameter, pass an array of Drive file IDs. Before each ID, specify the prefix `n`, for example: `"UF_TASK_WEBDAV_FILES": ["n428", "n345"]`. You can retrieve file identifiers in two ways.

Use one of the file upload methods:
  - [disk.storage.uploadfile](../disk/storage/disk-storage-upload-file.md)
  - [disk.folder.uploadfile](../disk/folder/disk-folder-upload-file.md)

Use one of the file list retrieval methods:
  - [disk.storage.getchildren](../disk/storage/disk-storage-get-children.md)
  - [disk.folder.getchildren](../disk/folder/disk-folder-get-children.md)

Attach files to a task using the [tasks.task.files.attach](./tasks-task-files-attach.md) method if the task has already been created.

{% note tip "Typical use-cases and scenarios" %}

- [{#T}](../../tutorials/tasks/how-to-create-task-with-file.md)
- [{#T}](../../tutorials/tasks/how-to-upload-file-to-task.md)

{% endnote %}

## Flows

Flows are a tool that automates task distribution and execution. Employees do not need to search for who will perform a task. They place tasks into a department flow, and it automatically assigns an executor.

Flows can be managed using the [tasks.flow.Flow.*](./flow/index.md) group of methods.

{% note tip "User documentation" %}

  - [Bitrix24 Flows: Getting Started](https://helpdesk.bitrix24.com/open/21415178/)

{% endnote %}

## Scrum tasks

Scrum tasks are standard Bitrix24 tasks with extended capabilities for working with the Scrum methodology. In Scrum, a team can:

  - estimate task complexity using story points
  - attach tasks to epics
  - place tasks in backlogs and sprints
  - move tasks through sprint stages during the work process

For more details about Scrum and its methods, see the article [Scrum: methods overview](../sonet-group/scrum/index.md).

{% note tip "User documentation" %}

  - [Bitrix24.Scrum](https://helpdesk.bitrix24.com/open/14786248/)

{% endnote %}

## Task operating modes

Kanban is a tool that helps visually represent task work in the form of columns and cards. Columns are work stages, and cards are tasks. Kanban is used for working with tasks in groups and projects.

"My plan" is a mode for managing your own tasks in a Kanban view. Each employee will have their own "My plan" stages.

Kanban and "My plan" stages can be managed using the [task.stages.*](./stages/index.md) group of methods.

## Tasks in "Daily plan"

"Daily plan" is a list of to-dos, tasks, and meetings that you have scheduled for the workday. The [task.planner.getlist](./planner/index.md) method retrieves the list of tasks from the "Daily plan".

## Widgets

An application can be embedded into a task card. Embedding allows you to use the application without leaving the card.

- [Tab in the task card](../widgets/task/view-tab.md) `TASK_VIEW_TAB`
- [Right panel of the task card](../widgets/task/view-sidebar.md) `TASK_VIEW_SIDEBAR`
- [Link in the top part of the task card](../widgets/task/view-top-panel.md) `TASK_VIEW_TOP_PANEL`

An application can also be embedded in the task list:

- [List context menu item](../widgets/task/index.md) `TASK_LIST_CONTEXT_MENU` 

In Kanban or "My Plan" task modes, there are two additional special embedding locations:

- [Main dropdown menu item](../widgets/task/list-toolbar.md) `TASK_USER_LIST_TOOLBAR`, `TASK_GROUP_LIST_TOOLBAR`
- [Main dropdown menu item near robot settings](../widgets/task/robot-designer-toolbar.md) `TASK_ROBOT_DESIGNER_TOOLBAR`


## Overview of Methods and Events {#all-methods}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: depends on the method

### Basic

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [tasks.task.add](./tasks-task-add.md) | Creates a task ||
    || [tasks.task.update](./tasks-task-update.md) | Updates a task ||
    || [tasks.task.get](./tasks-task-get.md) | Gets information about a task by `id` ||
    || [tasks.task.list](./tasks-task-list.md) | Gets a list of tasks ||
    || [tasks.task.files.attach](./tasks-task-files-attach.md) | Attaches files to a task ||
    || [tasks.task.delegate](./tasks-task-delegate.md) | Delegates tasks ||
    || [tasks.task.counters.get](./tasks-task-counters-get.md) | Gets user counters ||
    || [tasks.task.start](./tasks-task-start.md) | Moves a task to "in progress" status ||
    || [tasks.task.pause](./tasks-task-pause.md) | Stops task execution and moves it to "waiting" status ||
    || [tasks.task.defer](./tasks-task-defer.md) | Moves a task to "deferred" status ||
    || [tasks.task.complete](./tasks-task-complete.md) | Moves a task to "completed" status ||
    || [tasks.task.renew](./tasks-task-renew.md) | Renews a task after its completion ||
    || [tasks.task.approve](./tasks-task-approve.md) | Approves a task ||
    || [tasks.task.disapprove](./tasks-task-disapprove.md) | Rejects a task ||
    || [tasks.task.delete](./tasks-task-delete.md) | Deletes a task ||
    || [tasks.task.startwatch](./tasks-task-start-watch.md) | Allows watching a task ||
    || [tasks.task.stopwatch](./tasks-task-stop-watch.md) | Stops watching a task ||
    || [tasks.task.favorite.add](./tasks-task-favorite-add.md) | Adds tasks to favorites ||
    || [tasks.task.favorite.remove](./tasks-task-favorite-remove.md) | Removes tasks from favorites ||
    || [tasks.task.pin](./tasks-task-pin.md) | Pins a task in the list ||
    || [tasks.task.unpin](./tasks-task-unpin.md) | Unpins a task in the list ||
    || [tasks.task.getFields](./tasks-task-get-fields.md) | Gets available fields ||
    || [tasks.task.getaccess](./tasks-task-get-access.md) | Checks access to the task ||
    || [tasks.task.history.list](./tasks-task-history-list.md) | Gets the task history ||
    || [tasks.task.mute](./tasks-task-mute.md) | Enables "Mute" mode ||
    || [tasks.task.unmute](./tasks-task-unmute.md) | Disables "Mute" mode ||
    || [task.dependence.add](./task-dependence-add.md) | Creates a dependency of one task on another ||
    || [task.dependence.delete](./task-dependence-delete.md) | Deletes a dependency of one task on another ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [OnTaskAdd](./events-tasks/on-task-add.md) | On task addition ||
    || [OnTaskUpdate](./events-tasks/on-task-update.md) | On task update ||
    || [OnTaskDelete](./events-tasks/on-task-delete.md) | On task deletion ||
    |#

{% endlist %}

### Task result

#|
|| **Method** | **Description** ||
|| [tasks.task.result.addFromCommentt](./result/tasks-task-result-add-from-comment.md) | Adds a comment to the result ||
|| [tasks.task.result.list](./result/tasks-task-result-list.md) | Gets a list of task results ||
|| [tasks.task.result.deleteFromComment](./result/tasks-task-result-delete-from-comment.md) | Removes a comment from the task result ||
|#

### Checklists

#|
|| **Method** | **Description** ||
|| [task.checklistitem.add](./checklist-item/task-checklist-item-add.md) | Adds a new checklist item to the task ||
|| [task.checklistitem.update](./checklist-item/task-checklist-item-update.md) | Updates checklist item data ||
|| [task.checklistitem.get](./checklist-item/task-checklist-item-get.md) | Gets a checklist item by its `id` ||
|| [task.checklistitem.getlist](./checklist-item/task-checklist-item-get-list.md) | Gets a list of checklist items in the task ||
|| [task.checklistitem.moveafteritem](./checklist-item/task-checklist-item-move-after-item.md) | Places a checklist item in the list after the specified one ||
|| [task.checklistitem.complete](./checklist-item/task-checklist-item-complete.md) | Marks a checklist item as completed ||
|| [task.checklistitem.renew](./checklist-item/task-checklist-item-renew.md) | Marks a completed checklist item as active again ||
|| [task.checklistitem.delete](./checklist-item/task-checklist-item-delete.md) | Deletes a checklist item ||
|| [task.checklistitem.isactionallowed](./checklist-item/task-checklist-item-is-action-allowed.md) | Checks if an action is allowed for a checklist item ||
|| [task.checklistitem.getmanifest](./checklist-item/task-checklist-item-get-manifest.md) | Gets a list of methods and their descriptions ||
|#

### Comments

{% note warning %}

Comment methods are not applicable to the new task card. Task discussions are held in the task chat. For details, see the article [New task card: overview of changes](./tasks-new.md).

{% endnote %}

#|
|| **Method** | **Description** ||
|| [task.commentitem.add](./comment-item/task-comment-item-add.md) | Creates a new comment for a task ||
|| [task.commentitem.update](./comment-item/task-comment-item-update.md) | Updates comment data ||
|| [task.commentitem.get](./comment-item/task-comment-item-get.md) | Gets a comment for a task ||
|| [task.commentitem.getlist](./comment-item/task-comment-item-get-list.md) | Gets a list of comments for a task ||
|| [task.commentitem.delete](./comment-item/task-comment-item-delete.md) | Deletes a comment ||
|#

### Time spent

#|
|| **Method** | **Description** ||
|| [task.elapseditem.add](./elapsed-item/task-elapsed-item-add.md) | Adds time spent to a task ||
|| [task.elapseditem.update](./elapsed-item/task-elapsed-item-update.md) | Updates parameters of a time spent record ||
|| [task.elapseditem.get](./elapsed-item/task-elapsed-item-get.md) | Gets a time spent record by its identifier ||
|| [task.elapseditem.getlist](./elapsed-item/task-elapsed-item-get-list.md) | Gets a list of time spent records for a task ||
|| [task.elapseditem.delete](./elapsed-item/task-elapsed-item-delete.md) | Deletes a time spent record ||
|| [task.elapseditem.isactionallowed](./elapsed-item/task-elapsed-item-is-action-allowed.md) | Checks if an action is allowed ||
|| [task.elapseditem.getmanifest](./elapsed-item/task-elapsed-item-get-manifest.md) | Gets a list of methods and their descriptions ||
|#

### Custom fields

#|
|| **Method** | **Description** ||
|| [task.item.userfield.add](./user-field/task-item-user-field-add.md) | Creates a new field ||
|| [task.item.userfield.update](./user-field/task-item-user-field-update.md) | Updates field parameters ||
|| [task.item.userfield.get](./user-field/task-item-user-field-get.md) | Gets a field by identifier ||
|| [task.item.userfield.getlist](./user-field/task-item-user-field-get-list.md) | Gets a list of fields ||
|| [task.item.userfield.delete](./user-field/task-item-user-field-delete.md) | Deletes a field ||
|| [task.item.userfield.gettypes](./user-field/task-item-user-field-get-types.md) | Gets all available data types ||
|| [task.item.userfield.getfields](./user-field/task-item-user-field-get-fields.md) | Gets all available custom field types ||
|#

### Kanban and "My Plan" stages

#|
|| **Method** | **Description** ||
|| [task.stages.add](./stages/task-stages-add.md) | Adds Kanban or "My Plan" stages ||
|| [task.stages.update](./stages/task-stages-update.md) | Updates Kanban or "My Plan" stages ||
|| [task.stages.get](./stages/task-stages-get.md) | Gets Kanban or "My Plan" stages ||
|| [task.stages.canmovetask](./stages/task-stages-can-move-task.md) | Determines if the current user can move tasks in the specified object ||
|| [task.stages.movetask](./stages/task-stages-move-task.md) | Moves tasks from one stage to another ||
|| [task.stages.delete](./stages/task-stages-delete.md) | Deletes Kanban or "My Plan" stages ||
|#

### Tasks in "Plan for the day"

#|
|| **Method** | **Description** ||
|| [task.planner.getList](./planner/task-planner-get-list.md) | Gets a list of tasks from "Plan for the Day" ||
|#

### Flows

#|
|| **Method** | **Description** ||
|| [tasks.flow.Flow.create](./flow/tasks-flow-flow-create.md) | Create a stream ||
|| [tasks.flow.Flow.get](./flow/tasks-flow-flow-get.md) | Get a stream ||
|| [tasks.flow.Flow.update](./flow/tasks-flow-flow-update.md) | Change a stream ||
|| [tasks.flow.Flow.delete](./flow/tasks-flow-flow-delete.md) | Delete a stream ||
|| [tasks.flow.Flow.isExists](./flow/tasks-flow-flow-is-exists.md) | Check if a stream with such a name exists ||
|| [tasks.flow.Flow.activate](./flow/tasks-flow-flow-activate.md) | Enable or disable a stream ||
|#
