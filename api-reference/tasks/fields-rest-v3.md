# Task Fields in REST 3.0

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

This page helps you understand the task fields in REST 3.0. They are returned by the methods [tasks.task.get](./tasks-task-get-rest-v3.md), [tasks.task.list](./tasks-task-list-rest-v3.md), and [tasks.task.add](./tasks-task-add-rest-v3.md), and accepted by the methods [tasks.task.add](./tasks-task-add-rest-v3.md) and [tasks.task.update](./tasks-task-update-rest-v3.md). In the section [Task Object](#taskdto), all task fields are described, while other sections cover the fields of related objects.

Some task fields exist in two forms: as an identifier and as an object, for example, `creatorId` and `creator`. Which form each method accepts and returns is shown in the table.

## Which Fields the Methods Accept and Return {#methods}

#|
|| **Method** | **Accepts** | **Returns** ||
|| [tasks.task.add](./tasks-task-add-rest-v3.md) | Fields of the [Task Object](#taskdto) table with identifiers of related objects: `creatorId`, `responsibleId`, `groupId`. Required: `title`, `creatorId`, `responsibleId` | The task object with the same structure as [tasks.task.get](./tasks-task-get-rest-v3.md) without `select` ||
|| [tasks.task.update](./tasks-task-update-rest-v3.md) | The same fields as `tasks.task.add` | Success flag `true` ||
|| [tasks.task.get](./tasks-task-get-rest-v3.md) | Task `id` and `select` | Without `select`, the basic set of fields without related objects. List the object fields in `select` using dot notation, for example, `["creator.name", "creator.email"]`. The method does not return the identifiers of the creator, assignee, group, stage, flow, template, email, and change authors, even if they are specified in `select`. Such fields are marked in the table ||
|| [tasks.task.list](./tasks-task-list-rest-v3.md) | `select`, `filter`, `order`, `pagination` | Without `select`, only `id`. Returns the identifiers of related objects, but not the objects themselves, even if their fields are specified in `select` ||
|#

In `fields` of the methods `tasks.task.add` and `tasks.task.update`, you cannot pass `id`, `created`, `accomplices`, `auditors`, `tags`, `userFields`, and object fields — `creator`, `group`, `parent`, and others. For such a field, the method returns a validation error stating that the field is read-only. Fields marked "changes automatically" do not need to be passed: their value is calculated by Bitrix24.

Instructions on how to work with the fields of related objects are described in the article [Overview of REST API 3.0](../rest-v3.md#connection).

The rights to write and modify fields depend on the user's role in the task, group permission settings, employee hierarchy, task status, and task flags, such as `allowsChangeDeadline`. Before modifying a task, check the `rights` object: its fields show the actions available to the current user.

Boolean fields accept and return `true` or `false`. The string values `Y` and `N` from the classic task methods are not supported in REST 3.0: the method returns a validation error.

## Task Object {#taskdto}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Task identifier ||
|| **title**
[`string`](../data-types.md) | Task title, a required field for [creating a task](./tasks-task-add-rest-v3.md) ||
|| **description**
[`string`](../data-types.md) | Task description ||
|| **creatorId**
[`integer`](../data-types.md) | Creator identifier, a required field for [creating a task](./tasks-task-add-rest-v3.md). The field is returned by [tasks.task.list](./tasks-task-list-rest-v3.md); in the [tasks.task.get](./tasks-task-get-rest-v3.md) response, request the `creator` object instead ||
|| **creator**
[`object`](#user) | Creator. An object of type [user](#user). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **created**
[`datetime`](../data-types.md) | Creation date in ISO 8601 format. The field is returned by [tasks.task.list](./tasks-task-list-rest-v3.md); it is absent from the [tasks.task.get](./tasks-task-get-rest-v3.md) response ||
|| **responsibleId**
[`integer`](../data-types.md) | Assignee identifier, a required field for [creating a task](./tasks-task-add-rest-v3.md). The field is returned by [tasks.task.list](./tasks-task-list-rest-v3.md); in the [tasks.task.get](./tasks-task-get-rest-v3.md) response, request the `responsible` object instead ||
|| **responsible**
[`object`](#user) | Assignee. An object of type [user](#user). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **deadline**
[```datetime | null```](../data-types.md) | Deadline in ISO 8601 format, for example, `2025-12-31T23:59:59+02:00` ||
|| **needsControl**
[`boolean`](../data-types.md) | Task control by the creator: `true` — after the assignee completes the task, it waits for the creator's review. Default is `false` ||
|| **startPlan**
[```datetime | null```](../data-types.md) | Planned start date in ISO 8601 format, for example, `2025-12-31T06:00:00+02:00` ||
|| **endPlan**
[```datetime | null```](../data-types.md) | Planned end date in ISO 8601 format, for example, `2025-12-31T18:00:00+02:00` ||
|| **checklist**
[`array<integer>`](../data-types.md) | Identifiers of checklist items. To work with checklists, use the methods [task.checklistitem.*](./checklist-item/index.md) ||
|| **fileIds**
[```array<integer> | null```](../data-types.md) | Identifiers of Drive files to attach to the task. The field is accepted by [tasks.task.add](./tasks-task-add-rest-v3.md) and [tasks.task.update](./tasks-task-update-rest-v3.md). In the [tasks.task.get](./tasks-task-get-rest-v3.md) response, the field is not populated and is returned as `null` ||
|| **groupId**
[`integer`](../data-types.md) | Group/project identifier. To work with groups, use the methods [sonet_group.*](../sonet-group/index.md). The field is returned by [tasks.task.list](./tasks-task-list-rest-v3.md); in the [tasks.task.get](./tasks-task-get-rest-v3.md) response, request the `group` object instead ||
|| **group**
[`object`](#group) | Group/project. An object of type [group](#group). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **stageId**
[`integer`](../data-types.md) | Stage identifier. Use if the task is in a group/project. The field is returned by [tasks.task.list](./tasks-task-list-rest-v3.md); in the [tasks.task.get](./tasks-task-get-rest-v3.md) response, request the `stage` object instead ||
|| **stage**
[`object`](#stage) | Stage. An object of type [stage](#stage). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **epicId**
[```integer | null```](../data-types.md) | Epic identifier. To work with epics, use the methods [tasks.api.scrum.epic.*](../sonet-group/scrum/epic/index.md) ||
|| **storyPoints**
[```integer | null```](../data-types.md) | Story points. To update a Scrum task, use the method [tasks.api.scrum.task.update](../sonet-group/scrum/task/tasks-api-scrum-task-update.md) ||
|| **flowId**
[`integer`](../data-types.md) | Flow identifier. To work with flows, use the methods [tasks.flow.Flow.*](./flow/index.md). The field is returned by [tasks.task.list](./tasks-task-list-rest-v3.md); in the [tasks.task.get](./tasks-task-get-rest-v3.md) response, request the `flow` object instead ||
|| **flow**
[`object`](#flow) | Flow. An object of type [flow](#flow). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **priority**
[`string`](../data-types.md) | Task priority. Possible values:
- `high` — high
- `average` — average
- `low` — low ||
|| **status**
[`string`](../data-types.md) | Task status. Possible values:
- `pending` — waiting for execution
- `in_progress` — in progress
- `supposedly_completed` — awaiting control
- `completed` — completed
- `deferred` — deferred
- `declined` — declined ||
|| **statusChanged**
[```datetime | null```](../data-types.md) | Status change date in ISO 8601 format ||
|| **accomplices**
[`array<object>`](#user) | Participants. An array of objects of type [user](#user). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md), for example, `["accomplices.id", "accomplices.name"]`.

The methods [tasks.task.add](./tasks-task-add-rest-v3.md) and [tasks.task.update](./tasks-task-update-rest-v3.md) do not accept the field. To assign participants, use the classic method [tasks.task.update](./tasks-task-update.md) with the `ACCOMPLICES` field ||
|| **auditors**
[`array<object>`](#user) | Observers. An array of objects of type [user](#user). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md), for example, `["auditors.id", "auditors.name"]`.

The methods [tasks.task.add](./tasks-task-add-rest-v3.md) and [tasks.task.update](./tasks-task-update-rest-v3.md) do not accept the field. To assign observers, use the classic method [tasks.task.update](./tasks-task-update.md) with the `AUDITORS` field ||
|| **parentId**
[```integer | null```](../data-types.md) | Parent task identifier.
Has a value of `null` if there is no parent task ||
|| **parent**
[`object`](#taskdto) | Parent task. An object of type [task](#taskdto). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **containsChecklist**
[`boolean`](../data-types.md) | Indicates the presence of a checklist. The field is automatically updated ||
|| **containsSubTasks**
[`boolean`](../data-types.md) | Indicates the presence of subtasks. The field is automatically updated ||
|| **containsRelatedTasks**
[`boolean`](../data-types.md) | Indicates the presence of related tasks. The field is automatically updated ||
|| **containsGanttLinks**
[`boolean`](../data-types.md) | Indicates the presence of Gantt links. The field is automatically updated ||
|| **containsPlacements**
[`boolean`](../data-types.md) | Indicates the presence of integrations. The field is automatically updated ||
|| **containsResults**
[`boolean`](../data-types.md) | Indicates the presence of results. The field is automatically updated ||
|| **numberOfReminders**
[`integer`](../data-types.md) | Number of reminders for the task. The field is automatically updated ||
|| **chatId**
[`integer`](../data-types.md) | Task chat identifier. To work with the task chat, use the [chat message methods](../chats/messages/index.md). For more details, see [{#T}](./tasks-new.md) ||
|| **chat**
[`object`](#task-chat) | Task chat. An object of type [task chat](#task-chat). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **plannedDuration**
[`integer`](../data-types.md) | Planned duration ||
|| **actualDuration**
[`integer`](../data-types.md) | Actual duration ||
|| **durationType**
[`string`](../data-types.md) | Unit of planned duration. Possible values: `seconds`, `minutes`, `hours`, `days`, `weeks`, `months`, `years` ||
|| **started**
[```datetime | null```](../data-types.md) | Start date of execution in ISO 8601 format ||
|| **estimatedTime**
[`integer`](../data-types.md) | Time estimate in seconds ||
|| **replicate**
[`boolean`](../data-types.md) | Indicates a recurring task: `true` — the task repeats according to the template schedule ||
|| **changed**
[`datetime`](../data-types.md) | Change date in ISO 8601 format ||
|| **changedById**
[`integer`](../data-types.md) | Identifier of the user who changed the task. The field is returned by [tasks.task.list](./tasks-task-list-rest-v3.md); in the [tasks.task.get](./tasks-task-get-rest-v3.md) response, request the `changedBy` object instead ||
|| **changedBy**
[`object`](#user) | Who changed. An object of type [user](#user). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **statusChangedById**
[`integer`](../data-types.md) | Identifier of the user who changed the status. The field is returned by [tasks.task.list](./tasks-task-list-rest-v3.md); in the [tasks.task.get](./tasks-task-get-rest-v3.md) response, request the `statusChangedBy` object instead ||
|| **statusChangedBy**
[`object`](#user) | Who changed the status. An object of type [user](#user). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **closedById**
[`integer`](../data-types.md) | Identifier of the user who closed the task. The field is returned by [tasks.task.list](./tasks-task-list-rest-v3.md); in the [tasks.task.get](./tasks-task-get-rest-v3.md) response, request the `closedBy` object instead ||
|| **closedBy**
[`object`](#user) | Who closed. An object of type [user](#user). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **closed**
[```datetime | null```](../data-types.md) | Closing date in ISO 8601 format ||
|| **activity**
[`datetime`](../data-types.md) | Date of last activity in ISO 8601 format ||
|| **guid**
[`string`](../data-types.md) | Task `GUID` identifier ||
|| **xmlId**
[```string | null```](../data-types.md) | External identifier ||
|| **exchangeId**
[```string | null```](../data-types.md) | Exchange identifier ||
|| **exchangeModified**
[```string | null```](../data-types.md) | Date of modification in Exchange ||
|| **outlookVersion**
[`integer`](../data-types.md) | Version of synchronization with Outlook ||
|| **mark**
[`string`](../data-types.md) | Task rating. Possible values:
- `positive` — positive
- `negative` — negative
- `none` — unrated ||
|| **allowsChangeDeadline**
[`boolean`](../data-types.md) | The assignee is allowed to change the deadline ||
|| **allowsTimeTracking**
[`boolean`](../data-types.md) | Time tracking is enabled for the task ||
|| **matchesWorkTime**
[`boolean`](../data-types.md) | Consider working hours: skip weekends when calculating planned dates ||
|| **addInReport**
[```boolean | null```](../data-types.md) | Add the task to the report ||
|| **isMultitask**
[`boolean`](../data-types.md) | Indicates "base task with subtasks" ||
|| **siteId**
[`string`](../data-types.md) | Site identifier ||
|| **deadlineCount**
[```integer | null```](../data-types.md) | Service field of task counters. Do not use it in integrations ||
|| **declineReason**
[```string | null```](../data-types.md) | Reason for declining the task. Populated when the assignee has declined the task ||
|| **forumTopicId**
[```integer | null```](../data-types.md) | Identifier of the forum topic with task comments. The value is `null` until the topic is created ||
|| **forkedByTemplateId**
[`integer`](../data-types.md) | Identifier of the template if the task was created from a template. The field is returned by [tasks.task.list](./tasks-task-list-rest-v3.md); in the [tasks.task.get](./tasks-task-get-rest-v3.md) response, request the `forkedByTemplate` object instead ||
|| **forkedByTemplate**
[`object`](#template) | Task template. An object of type [template](#template). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **maxDeadlineChangeDate**
[```datetime | null```](../data-types.md) | Date after which the deadline cannot be changed, in ISO 8601 format ||
|| **maxDeadlineChanges**
[```integer | null```](../data-types.md) | Maximum number of deadline extensions ||
|| **requireDeadlineChangeReason**
[`boolean`](../data-types.md) | Require a reason when the deadline is moved ||
|| **tags**
[`array<object>`](#tag) | Task tags. An array of objects of type [tag](#tag). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md), for example, `["tags.id", "tags.name"]` ||
|| **link**
[`string`](../data-types.md) | Relative link to the task in the Bitrix24 interface, for example, `/company/personal/user/1/tasks/task/view/289/` ||
|| **userFields**
[`array<object>`](#user-field) | Custom fields of the task. An array of objects of type [custom field](#user-field). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md), for example, `["userFields.key", "userFields.value"]` ||
|| **rights**
[`object`](../data-types.md) | Actions of the current user with the task. The key is the action code, and the value is `true` if the action is available. For example, `edit` — edit the task, `complete` — complete, `delegate` — delegate, `changeResponsible` — change the assignee. The full set of keys is in the response example of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **archiveLink**
[`string`](../data-types.md) | Link to the archive for downloading all task files ||
|| **crmItemIds**
[`array<string>`](../data-types.md) | Identifiers of related CRM objects in the format:
- `L_XX` — lead
- `D_XX` — deal
- `C_XX` — contact
- `CO_XX` — company
- `SI_XX` — invoice
- `TXX_XX` — SPA ||
|| **emailId**
[`integer`](../data-types.md) | Identifier of the email from which the task was created. The field is returned by [tasks.task.list](./tasks-task-list-rest-v3.md); in the [tasks.task.get](./tasks-task-get-rest-v3.md) response, request the `email` object instead ||
|| **email**
[`object`](#email) | Email from which the task was created. An object of type [email](#email). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **elapsedTime**
[`object`](#elapsed-time) | Time tracking. An object of type [time tracking](#elapsed-time). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **requireResult**
[`boolean`](../data-types.md) | Require a result: the task cannot be completed without a result record ||
|| **matchesSubTasksTime**
[`boolean`](../data-types.md) | Consider subtask deadlines when calculating planned dates ||
|| **autocompleteSubTasks**
[`boolean`](../data-types.md) | Complete subtasks automatically together with the base task ||
|| **allowsChangeDatePlan**
[`boolean`](../data-types.md) | The assignee is allowed to change planned dates ||
|| **inFavorite**
[`array<integer>`](../data-types.md) | Indicates "in favorites". The field returns an array containing the ID of the current user if their setting is active `"inFavorite": [29]` ||
|| **inPin**
[`array<integer>`](../data-types.md) | Indicates "task pinned". The field returns an array containing the ID of the current user if their setting is active `"inPin": [29]` ||
|| **inGroupPin**
[`array<integer>`](../data-types.md) | Indicates "task pinned in group". The field returns an array containing the ID of the current user if their setting is active `"inGroupPin": [29]` ||
|| **inMute**
[`array<integer>`](../data-types.md) | Indicates "mute". The field returns an array containing the ID of the current user if their setting is active `"inMute": [29]` ||
|| **source**
[`object`](#source) | Source of the task. An object [source](#source). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **dependsOn**
[`array`](../data-types.md) | Dependencies on tasks ||
|| **scenarios**
[`array<string>`](../data-types.md) | Task creation scenarios. Possible element values:
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
[`integer`](../data-types.md) | User identifier ||
|| **name**
[`string`](../data-types.md) | User name ||
|| **role**
[`string`](../data-types.md) | User role ||
|| **image**
[`object`](#file) | An object of type [file](#file). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **gender**
[`string`](../data-types.md) | Gender. Possible values:
- `M` — male
- `F` — female
- `N` — not specified ||
|| **email**
[`string`](../data-types.md) | Email ||
|| **externalAuthId**
[`string`](../data-types.md) | External auth ID ||
|| **rights**
[`array`](../data-types.md) | User rights ||
|#

## Tag Object {#tag}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Tag identifier ||
|| **name**
[`string`](../data-types.md) | Tag name ||
|#

## Custom Field Object {#user-field}

Custom fields of a task are created by the methods [task.item.userfield.*](./user-field/index.md).

#|
|| **Name**
`type` | **Description** ||
|| **key**
[`string`](../data-types.md) | Field code, for example, `UF_CRM_TASK` ||
|| **value**
[`any`](../data-types.md) | Field value. The type depends on the field settings. If the field is empty — `null` ||
|#

## File Object {#file}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | File identifier ||
|| **src**
[`string`](../data-types.md) | Link to the file ||
|| **name**
[`string`](../data-types.md) | File name ||
|| **width**
[`integer`](../data-types.md) | Width ||
|| **height**
[`integer`](../data-types.md) | Height ||
|| **size**
[`integer`](../data-types.md) | Size ||
|| **subDir**
[`string`](../data-types.md) | Subdirectory ||
|| **contentType**
[`string`](../data-types.md) | MIME type ||
|| **file**
[`array`](../data-types.md) | File data ||
|#

## Group Object {#group}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Group identifier ||
|| **name**
[`string`](../data-types.md) | Group name ||
|| **image**
[`object`](#file) | An object of type [file](#file). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **type**
[`string`](../data-types.md) | Group type. Possible values:
- `group` — group
- `project` — project
- `scrum` — Scrum
- `collab` — collab ||
|| **isVisible**
[`boolean`](../data-types.md) | Visibility indicator ||
|#

## Stage Object {#stage}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Stage identifier ||
|| **title**
[`string`](../data-types.md) | Stage title ||
|| **color**
[`string`](../data-types.md) | Stage color ||
|#

## Flow Object {#flow}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Flow identifier ||
|| **name**
[`string`](../data-types.md) | Flow name ||
|#

## Task Chat Object {#task-chat}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Chat element identifier ||
|| **entityId**
[`integer`](../data-types.md) | Chat object identifier ||
|| **entityType**
[`string`](../data-types.md) | Chat object type. For a task chat — `TASKS_TASK` ||
|#

## Task Template Object {#template}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Template identifier ||
|| **task**
[`object`](#taskdto) | An object of type [task](#taskdto). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **title**
[`string`](../data-types.md) | Title ||
|| **description**
[`string`](../data-types.md) | Description ||
|| **creator**
[`object`](#user) | An object of type [user](#user). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **responsibleCollection**
[`array`](../data-types.md) | Collection of responsible persons ||
|| **deadlineAfterTs**
[`integer`](../data-types.md) | Deadline shift ||
|| **startDatePlanTs**
[`integer`](../data-types.md) | Planned start date ||
|| **endDatePlanTs**
[`integer`](../data-types.md) | Planned end date ||
|| **replicate**
[`boolean`](../data-types.md) | Task repetition from the template ||
|| **checklist**
[`array`](../data-types.md) | Array of checklist item identifiers ||
|| **group**
[`object`](#group) | An object of type [group](#group). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|| **priority**
[`string`](../data-types.md) | Priority ||
|| **accomplices**
[`array`](../data-types.md) | Participants ||
|| **auditors**
[`array`](../data-types.md) | Auditors ||
|| **parent**
[`object`](#template) | Parent template. An object of type [task template](#template) ||
|| **replicateParams**
[`object`](#template-replicate-params) | An object of [replication parameters](#template-replicate-params). Use for data requests in the `select` parameter of [tasks.task.get](./tasks-task-get-rest-v3.md) ||
|#

## Template Replication Parameters Object {#template-replicate-params}

#|
|| **Name**
`type` | **Description** ||
|| **period**
[`string`](../data-types.md) | Frequency. Possible values:
- `daily` — daily
- `weekly` — weekly
- `monthly` — monthly
- `yearly` — yearly ||
|| **everyDay**
[`string`](../data-types.md) | Every day ||
|| **workdayOnly**
[`string`](../data-types.md) | Only working days ||
|| **dailyMonthInterval**
[`string`](../data-types.md) | Interval in days of the month ||
|| **everyWeek**
[`string`](../data-types.md) | Every week ||
|| **monthlyType**
[`string`](../data-types.md) | Type of monthly repetition ||
|| **monthlyDayNum**
[`string`](../data-types.md) | Day of the month ||
|| **monthlyMonthNum1**
[`string`](../data-types.md) | First month of the period ||
|| **monthlyWeekDayNum**
[`string`](../data-types.md) | Week number in the month ||
|| **monthlyWeekDay**
[`string`](../data-types.md) | Day of the week ||
|| **monthlyMonthNum2**
[`string`](../data-types.md) | Second month of the period ||
|| **yearlyType**
[`string`](../data-types.md) | Type of yearly repetition ||
|| **yearlyDayNum**
[`string`](../data-types.md) | Day of the month for yearly repetition ||
|| **yearlyMonth1**
[`string`](../data-types.md) | First month of yearly repetition ||
|| **yearlyWeekDayNum**
[`string`](../data-types.md) | Week number for yearly repetition ||
|| **yearlyWeekDay**
[`string`](../data-types.md) | Day of the week for yearly repetition ||
|| **yearlyMonth2**
[`string`](../data-types.md) | Second month of yearly repetition ||
|| **time**
[`string`](../data-types.md) | Time ||
|| **timezoneOffset**
[`string`](../data-types.md) | Timezone offset ||
|| **startDate**
[`string`](../data-types.md) | Start date of repetition ||
|| **repeatTill**
[`string`](../data-types.md) | Until what date to repeat ||
|| **endDate**
[`string`](../data-types.md) | End date of repetition ||
|| **times**
[`string`](../data-types.md) | Number of repetitions ||
|#

## Email Object {#email}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Email ID ||
|| **taskId**
[`integer`](../data-types.md) | Task ID ||
|| **mailboxId**
[`integer`](../data-types.md) | Mailbox ID ||
|| **title**
[`string`](../data-types.md) | Email title ||
|| **body**
[`string`](../data-types.md) | Email body ||
|| **from**
[`string`](../data-types.md) | Email sender ||
|| **dateTs**
[`integer`](../data-types.md) | Email sending timestamp ||
|| **link**
[`string`](../data-types.md) | Link to the email ||
|#

## Time Tracking Object {#elapsed-time}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Time tracking record identifier ||
|| **userId**
[`integer`](../data-types.md) | User ||
|| **taskId**
[`integer`](../data-types.md) | Task ||
|| **minutes**
[`integer`](../data-types.md) | Minutes ||
|| **seconds**
[`integer`](../data-types.md) | Seconds ||
|| **source**
[`string`](../data-types.md) | Source of the time-tracking entry. Possible values:
- `manual` — the employee entered the time manually
- `system` — the time was recorded by the task timer
- `unknown` — the source is not determined ||
|| **text**
[`string`](../data-types.md) | Comment ||
|| **createdAtTs**
[`integer`](../data-types.md) | Creation date ||
|| **startTs**
[`integer`](../data-types.md) | Start time ||
|| **stopTs**
[`integer`](../data-types.md) | End time ||
|#

## Source Object {#source}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../data-types.md) | Source type. Possible value — `chat`: the task was created from a chat message ||
|| **data**
[`array`](../data-types.md) | Source data ||
|#

## Continue Learning

- [{#T}](./tasks-task-get-rest-v3.md)
- [{#T}](./tasks-task-list-rest-v3.md)
- [{#T}](./tasks-task-add-rest-v3.md)
- [{#T}](./tasks-task-update-rest-v3.md)
- [{#T}](../rest-v3.md)
