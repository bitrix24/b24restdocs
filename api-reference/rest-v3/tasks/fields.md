# Task Fields in REST 3.0

In the section [Task Object](#taskdto), all task fields are described, while other sections cover the fields of related objects. Some task fields are available as both an identifier number and as an object, for example, `creatorId` and `creator`. Use the identifier field in the methods for [creating](./tasks-task-add.md) and [updating](./tasks-task-update.md) a task. Use the object field in the method for [getting a task](./tasks-task-get.md). Instructions on how to work with the fields of related objects are described in the article [Overview of REST API 3.0](../index.md#connection).

The rights to write and modify fields depend on the user's role in the task, group permission settings, employee hierarchy, task status, and certain flags in the task, such as `allowChangeDeadline`.

## Task Object {#taskdto}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Task identifier ||
|| **title**
[`string`](../../data-types.md) | Task title, a required field for [creating a task](./tasks-task-add.md) ||
|| **description**
[`string`](../../data-types.md) | Task description ||
|| **creatorId**
[`integer`](../../data-types.md) | Creator identifier, a required field for [creating a task](./tasks-task-add.md) ||
|| **creator**
[`object`](#user) | Creator. An object of type [user](#user). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **created**
[`datetime`](../../data-types.md) | Creation date in ISO-8601 format ||
|| **responsibleId**
[`integer`](../../data-types.md) | Assignee identifier, a required field for [creating a task](./tasks-task-add.md) ||
|| **responsible**
[`object`](#user) | Assignee. An object of type [user](#user). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **deadline**
[`datetime`](../../data-types.md) | Deadline in ISO-8601 format, for example, `2025-12-31T23:59:59+02:00` ||
|| **needsControl**
[`boolean`](../../data-types.md) | Task control by the creator. Possible values:
`Y` — enabled
`N` — disabled, default value ||
|| **startPlan**
[`datetime`](../../data-types.md) | Planned start date in ISO-8601 format, for example, `2025-12-31T06:00:00+02:00` ||
|| **endPlan**
[`datetime`](../../data-types.md) | Planned end date in ISO-8601 format, for example, `2025-12-31T18:00:00+02:00` ||
|| **checklist**
[`array`](../../data-types.md) | Identifiers of checklist items. To work with checklists, use the methods [task.checklistitem.*](../../tasks/checklist-item/index.md) ||
|| **groupId**
[`integer`](../../data-types.md) | Group/project identifier. To work with groups, use the methods [sonet_group.*](../../sonet-group/index.md) ||
|| **group**
[`object`](#group) | Group/project. An object of type [group](#group). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **stageId**
[`integer`](../../data-types.md) | Stage identifier. Use if the task is in a group/project ||
|| **stage**
[`object`](#stage) | Stage. An object of type [stage](#stage). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **epicId**
[`integer`](../../data-types.md) | Epic identifier. To work with epics, use the methods [tasks.api.scrum.epic.*](../../sonet-group/scrum/epic/index.md) ||
|| **storyPoints**
[`integer`](../../data-types.md) | Story points. To update a Scrum task, use the method [tasks.api.scrum.task.update](../../sonet-group/scrum/task/tasks-api-scrum-task-update.md) ||
|| **flowId**
[`integer`](../../data-types.md) | Flow identifier. To work with flows, use the methods [tasks.flow.Flow.*](../../tasks/flow/index.md) ||
|| **flow**
[`object`](#flow) | Flow. An object of type [flow](#flow). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **priority**
[`string`](../../data-types.md) | Task priority. Possible values:
- `high` — high
- `average` — average
- `low` — low ||
|| **status**
[`string`](../../data-types.md) | Task status. Possible values:
- `pending` — waiting for execution
- `in_progress` — in progress
- `supposedly_completed` — awaiting control
- `completed` — completed
- `deferred` — deferred
- `declined` — declined ||
|| **statusChanged**
[`datetime`](../../data-types.md) | Status change date in ISO 8601 format ||
|| **accomplices**
[`array<object>`](#user) | List of user identifiers — participants in the methods for [creating](./tasks-task-add.md) or [updating](./tasks-task-update.md) a task.
An array of objects of type [user](#user). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **auditors**
[`array<object>`](#user) | List of user identifiers — auditors of the task in the methods for [creating](./tasks-task-add.md) or [updating](./tasks-task-update.md) a task.
An array of objects of type [user](#user). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **parentId**
[`integer`](../../data-types.md) | Parent task identifier.
Has a value of `null` if there is no parent task ||
|| **parent**
[`object`](#taskdto) | Parent task. An object of type [task](#taskdto). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **containsChecklist**
[`boolean`](../../data-types.md) | Indicates the presence of a checklist. The field is automatically updated ||
|| **containsSubTasks**
[`boolean`](../../data-types.md) | Indicates the presence of subtasks. The field is automatically updated ||
|| **containsRelatedTasks**
[`boolean`](../../data-types.md) | Indicates the presence of related tasks. The field is automatically updated ||
|| **containsGanttLinks**
[`boolean`](../../data-types.md) | Indicates the presence of Gantt links. The field is automatically updated ||
|| **containsPlacements**
[`boolean`](../../data-types.md) | Indicates the presence of integrations. The field is automatically updated ||
|| **containsResults**
[`boolean`](../../data-types.md) | Indicates the presence of results. The field is automatically updated ||
|| **numberOfReminders**
[`integer`](../../data-types.md) | Number of reminders for the task. The field is automatically updated ||
|| **chatId**
[`integer`](../../data-types.md) | Task chat identifier. To work with the task chat, use the methods [im.message.*](../../tasks/tasks-new.md#comments) ||
|| **chat**
[`object`](#task-chat) | Task chat. An object of type [task chat](#task-chat). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **plannedDuration**
[`integer`](../../data-types.md) | Planned duration ||
|| **actualDuration**
[`integer`](../../data-types.md) | Actual duration ||
|| **durationType**
[`string`](../../data-types.md) | Unit of planned duration. Possible values: `secs`, `mins`, `hours`, `days`, `weeks`, `monts`, `years` ||
|| **started**
[`datetime`](../../data-types.md) | Start date of execution in ISO 8601 format ||
|| **estimatedTime**
[`integer`](../../data-types.md) | Time estimate in seconds ||
|| **replicate**
[`boolean`](../../data-types.md) | Indicates a recurring task. Possible values: 
- `Y` — yes, make the task recurring
- `N` — do not repeat ||
|| **changed**
[`datetime`](../../data-types.md) | Change date in ISO 8601 format ||
|| **changedById**
[`integer`](../../data-types.md) | Identifier of the user who changed the task ||
|| **changedBy**
[`object`](#user) | Who changed. An object of type [user](#user). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **statusChangedById**
[`integer`](../../data-types.md) | Identifier of the user who changed the status ||
|| **statusChangedBy**
[`object`](#user) | Who changed the status. An object of type [user](#user). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **closedById**
[`integer`](../../data-types.md) | Identifier of the user who closed the task ||
|| **closedBy**
[`object`](#user) | Who closed. An object of type [user](#user). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **closed**
[`datetime`](../../data-types.md) | Closing date in ISO 8601 format ||
|| **activity**
[`datetime`](../../data-types.md) | Date of last activity in ISO 8601 format ||
|| **guid**
[`string`](../../data-types.md) | Task `GUID` identifier ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier ||
|| **exchangeId**
[`string`](../../data-types.md) | Exchange identifier ||
|| **exchangeModified**
[`string`](../../data-types.md) | Date of modification in Exchange ||
|| **outlookVersion**
[`integer`](../../data-types.md) | Version of synchronization with Outlook ||
|| **mark**
[`string`](../../data-types.md) | Task rating. Possible values:
`N` — negative
`P` — positive
`null` — unrated ||
|| **allowsChangeDeadline**
[`boolean`](../../data-types.md) | Allowed to change the deadline. Possible values: 
- `Y` — allowed
- `N` — not allowed ||
|| **allowsTimeTracking**
[`boolean`](../../data-types.md) | Time tracking enabled for the task. Possible values: 
- `Y` — enabled
- `N` — not enabled ||
|| **matchesWorkTime**
[`boolean`](../../data-types.md) | Consider working hours. Possible values: 
- `Y` — yes
- `N` — no ||
|| **addInReport**
[`boolean`](../../data-types.md) | Add to report. Possible values: 
- `Y` — add 
- `N` — do not add ||
|| **isMultitask**
[`boolean`](../../data-types.md) | Indicates "base task with subtasks". Possible values: 
- `Y` — yes
- `N` — no ||
|| **siteId**
[`string`](../../data-types.md) | Site identifier ||
|| **forkedByTemplateId**
[`integer`](../../data-types.md) | Identifier of the template if the task was created from a template ||
|| **forkedByTemplate**
[`object`](#template) | Task template. An object of type [template](#template). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **maxDeadlineChangeDate**
[`datetime`](../../data-types.md) | Date after which the deadline cannot be changed, in ISO 8601 format ||
|| **maxDeadlineChanges**
[`integer`](../../data-types.md) | Maximum number of deadline extensions ||
|| **requireDeadlineChangeReason**
[`boolean`](../../data-types.md) | Require a reason when changing the deadline. Possible values: 
- `Y` — yes
- `N` — no ||
|| **link**
[`string`](../../data-types.md) | Link to the task ||
|| **rights**
[`array`](../../data-types.md) | Array of actions that the user can perform with the task ||
|| **archiveLink**
[`string`](../../data-types.md) | Link to the archive for downloading all task files ||
|| **crmItemIds**
[`array`](../../data-types.md) | Array of identifiers of related CRM objects in the format:
- `L_XX` — lead,
- `D_XX` — deal
- `C_XX` — contact
- `CO_XX` — company
- `SI_XX` — invoice
- `TXX_XX` — SPA ||
|| **emailId**
[`integer`](../../data-types.md) | Identifier of the email from which the task was created ||
|| **email**
[`object`](#email) | Email from which the task was created. An object of type [email](#email). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **elapsedTime**
[`object`](#elapsed-time) | Time tracking. An object of type [time tracking](#elapsed-time). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **requireResult**
[`boolean`](../../data-types.md) | Require a result. Possible values: 
- `Y` — yes
- `N` — no  ||
|| **matchesSubTasksTime**
[`boolean`](../../data-types.md) | Consider subtasks deadlines. Possible values: 
- `Y` — yes
- `N` — no  ||
|| **autocompleteSubTasks**
[`boolean`](../../data-types.md) | Auto-completion of subtasks. Possible values: 
- `Y` — yes
- `N` — no  ||
|| **allowsChangeDatePlan**
[`boolean`](../../data-types.md) | Allowed to change planned dates. Possible values: 
- `Y` — yes
- `N` — no  ||
|| **inFavorite**
[`array`](../../data-types.md) | Indicates "in favorites". The field returns an array containing the ID of the current user if their setting is active `"inFavorite": [29]` ||
|| **inPin**
[`array`](../../data-types.md) | Indicates "task pinned". The field returns an array containing the ID of the current user if their setting is active `"inPin": [29]` ||
|| **inGroupPin**
[`array`](../../data-types.md) | Indicates "task pinned in group". The field returns an array containing the ID of the current user if their setting is active `"inGroupPin": [29]` ||
|| **inMute**
[`array`](../../data-types.md) | Indicates "mute". The field returns an array containing the ID of the current user if their setting is active `"inMute": [29]` ||
|| **source**
[`object`](#source) | Source of the task. An object [source](#source). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **dependsOn**
[`array`](../../data-types.md) | Dependencies on tasks ||
|| **scenarios**
[`array`](../../data-types.md) | Task creation scenario. Possible values: 
- `default` — default value
- `crm` — CRM
- `mobile` — mobile application
- `voice` — audio task AI
- `video` — video task AI ||
|#

## User Object {#user}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | User identifier ||
|| **name**
[`string`](../../data-types.md) | User name ||
|| **role**
[`string`](../../data-types.md) | User role ||
|| **image**
[`object`](#file) | An object of type [file](#file). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **gender**
[`string`](../../data-types.md) | Gender ||
|| **email**
[`string`](../../data-types.md) | Email ||
|| **externalAuthId**
[`string`](../../data-types.md) | External auth ID ||
|| **rights**
[`array`](../../data-types.md) | User rights ||
|#

## File Object {#file}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | File identifier ||
|| **src**
[`string`](../../data-types.md) | Link to the file ||
|| **name**
[`string`](../../data-types.md) | File name ||
|| **width**
[`integer`](../../data-types.md) | Width ||
|| **height**
[`integer`](../../data-types.md) | Height ||
|| **size**
[`integer`](../../data-types.md) | Size ||
|| **subDir**
[`string`](../../data-types.md) | Subdirectory ||
|| **contentType**
[`string`](../../data-types.md) | MIME type ||
|| **file**
[`array`](../../data-types.md) | File data ||
|#

## Group Object {#group}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Group identifier ||
|| **name**
[`string`](../../data-types.md) | Group name ||
|| **image**
[`object`](#file) | An object of type [file](#file). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **type**
[`string`](../../data-types.md) | Group type ||
|| **isVisible**
[`boolean`](../../data-types.md) | Visibility indicator ||
|#

## Stage Object {#stage}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Stage identifier ||
|| **title**
[`string`](../../data-types.md) | Stage title ||
|| **color**
[`string`](../../data-types.md) | Stage color ||
|#

## Flow Object {#flow}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Flow identifier ||
|| **name**
[`string`](../../data-types.md) | Flow name ||
|#

## Task Chat Object {#task-chat}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Chat element identifier ||
|| **entityId**
[`integer`](../../data-types.md) | Chat object identifier ||
|| **entityType**
[`string`](../../data-types.md) | Chat object type ||
|#

## Task Template Object {#template}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Template identifier ||
|| **task**
[`object`](#taskdto) | An object of type [task](#taskdto). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **title**
[`string`](../../data-types.md) | Title ||
|| **description**
[`string`](../../data-types.md) | Description ||
|| **creator**
[`object`](#user) | An object of type [user](#user). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **responsibleCollection**
[`array`](../../data-types.md) | Collection of responsible persons ||
|| **deadlineAfterTs**
[`integer`](../../data-types.md) | Deadline shift ||
|| **startDatePlanTs**
[`integer`](../../data-types.md) | Planned start date ||
|| **endDatePlanTs**
[`integer`](../../data-types.md) | Planned end date ||
|| **replicate**
[`boolean`](../../data-types.md) | Task repetition from the template ||
|| **checklist**
[`array`](../../data-types.md) | Array of checklist item identifiers ||
|| **group**
[`object`](#group) | An object of type [group](#group). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|| **priority**
[`string`](../../data-types.md) | Priority ||
|| **accomplices**
[`array`](../../data-types.md) | Participants ||
|| **auditors**
[`array`](../../data-types.md) | Auditors ||
|| **parent**
[`object`](#template) | Parent template. An object of type [task template](#template) ||
|| **replicateParams**
[`object`](#template-replicate-params) | An object of [replication parameters](#template-replicate-params). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get.md) ||
|#

## Template Replication Parameters Object {#template-replicate-params}

#|
|| **Name**
`type` | **Description** ||
|| **period**
[`string`](../../data-types.md) | Frequency ||
|| **everyDay**
[`string`](../../data-types.md) | Every day ||
|| **workdayOnly**
[`string`](../../data-types.md) | Only working days ||
|| **dailyMonthInterval**
[`string`](../../data-types.md) | Interval in days of the month ||
|| **everyWeek**
[`string`](../../data-types.md) | Every week ||
|| **monthlyType**
[`string`](../../data-types.md) | Type of monthly repetition ||
|| **monthlyDayNum**
[`string`](../../data-types.md) | Day of the month ||
|| **monthlyMonthNum1**
[`string`](../../data-types.md) | First month of the period ||
|| **monthlyWeekDayNum**
[`string`](../../data-types.md) | Week number in the month ||
|| **monthlyWeekDay**
[`string`](../../data-types.md) | Day of the week ||
|| **monthlyMonthNum2**
[`string`](../../data-types.md) | Second month of the period ||
|| **yearlyType**
[`string`](../../data-types.md) | Type of yearly repetition ||
|| **yearlyDayNum**
[`string`](../../data-types.md) | Day of the month for yearly repetition ||
|| **yearlyMonth1**
[`string`](../../data-types.md) | First month of yearly repetition ||
|| **yearlyWeekDayNum**
[`string`](../../data-types.md) | Week number for yearly repetition ||
|| **yearlyWeekDay**
[`string`](../../data-types.md) | Day of the week for yearly repetition ||
|| **yearlyMonth2**
[`string`](../../data-types.md) | Second month of yearly repetition ||
|| **time**
[`string`](../../data-types.md) | Time ||
|| **timezoneOffset**
[`string`](../../data-types.md) | Timezone offset ||
|| **startDate**
[`string`](../../data-types.md) | Start date of repetition ||
|| **repeatTill**
[`string`](../../data-types.md) | Until what date to repeat ||
|| **endDate**
[`string`](../../data-types.md) | End date of repetition ||
|| **times**
[`string`](../../data-types.md) | Number of repetitions ||
|#

## Email Object {#email}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Email ID ||
|| **taskId**
[`integer`](../../data-types.md) | Task ID ||
|| **mailboxId**
[`integer`](../../data-types.md) | Mailbox ID ||
|| **title**
[`string`](../../data-types.md) | Email title ||
|| **body**
[`string`](../../data-types.md) | Email body ||
|| **from**
[`string`](../../data-types.md) | Email sender ||
|| **dateTs**
[`integer`](../../data-types.md) | Email sending timestamp ||
|| **link**
[`string`](../../data-types.md) | Link to the email ||
|#

## Time Tracking Object {#elapsed-time}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Time tracking record identifier ||
|| **userId**
[`integer`](../../data-types.md) | User ||
|| **taskId**
[`integer`](../../data-types.md) | Task ||
|| **minutes**
[`integer`](../../data-types.md) | Minutes ||
|| **seconds**
[`integer`](../../data-types.md) | Seconds ||
|| **source**
[`string`](../../data-types.md) | Source ||
|| **text**
[`string`](../../data-types.md) | Comment ||
|| **createdAtTs**
[`integer`](../../data-types.md) | Creation date ||
|| **startTs**
[`integer`](../../data-types.md) | Start time ||
|| **stopTs**
[`integer`](../../data-types.md) | End time ||
|#

## Source Object {#source}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../data-types.md) | Source type ||
|| **data**
[`array`](../../data-types.md) | Source data ||
|#