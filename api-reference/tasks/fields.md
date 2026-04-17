# Task Fields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The recording and modification of fields are carried out according to business logic and user permissions. This means it depends on the user's role, group access permissions, hierarchy, task status, and certain flags in the task, such as `allowChangeDeadline`.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../data-types.md) | Task identifier ||
|| **parentId**
[`string`](../data-types.md) | Identifier of the parent task.

It is `null` if there is no parent task ||
|| **title**
[`string`](../data-types.md) | Task title ||
|| **description**
[`string`](../data-types.md) | Task description ||
|| **chatId**
[`integer`](../data-types.md) | Identifier of the chat for the [new task card](tasks-new.md) ||
|| **mark**
[`string`](../data-types.md) | Task rating. Possible values:
`N` — negative
`P` — positive
`null` — no rating ||
|| **priority**
[`string`](../data-types.md) | Task priority. Possible values:
- `2` — high
- `1` — medium
- `0` — low ||
|| **multitask**
[`string`](../data-types.md) | Indicator of "base task with subtasks." Possible values: 
- `Y` — yes
- `N` — no  ||
|| **notViewed**
[`string`](../data-types.md) | Indicator of "not viewed." Possible values: 
- `Y` — not viewed
- `N` — viewed  ||
|| **replicate**
[`string`](../data-types.md) | Indicator of "repeat task." Possible values: 
- `Y` — yes, make the task recurring
- `N` — do not repeat ||
|| **stageId**
[`string`](../data-types.md) | Stage identifier ||
|| **sprintId**
[`string`](../data-types.md) | Sprint identifier ||
|| **backlogId**
[`string`](../data-types.md) | Backlog identifier ||
|| **createdBy**
[`string`](../data-types.md) | Identifier of the Creator ||
|| **createdDate**
[`string`](../data-types.md) | Creation date in `ISO 8601` format ||
|| **responsibleId**
[`string`](../data-types.md) | Identifier of the responsible person ||
|| **changedBy**
[`string`](../data-types.md) | Identifier of the user who modified the task ||
|| **changedDate**
[`string`](../data-types.md) | Modification date in `ISO 8601` format ||
|| **statusChangedBy**
[`string`](../data-types.md) | Identifier of the user who changed the task status ||
|| **closedBy**
[`string`](../data-types.md) | Identifier of the user who completed the task. It is `0` if the task is not completed ||
|| **closedDate**
[`string`](../data-types.md) | Completion date in `ISO 8601` format. It is `null` if the task is not completed ||
|| **activityDate**
[`string`](../data-types.md) | Date of last activity in `ISO 8601` format ||
|| **dateStart**
[`string`](../data-types.md) | Start date ||
|| **deadline**
[`string`](../data-types.md) | Deadline ||
|| **startDatePlan**
[`string`](../data-types.md) | Planned start date.

In the [create](./tasks-task-add.md) and [update](./tasks-task-update.md) methods, `endDatePlan` must be passed along with `startDatePlan` ||
|| **endDatePlan**
[`string`](../data-types.md) | Planned end date.

In the [create](./tasks-task-add.md) and [update](./tasks-task-update.md) methods, `startDatePlan` must be passed along with `endDatePlan` ||
|| **guid**
[`string`](../data-types.md) | Task `GUID` identifier ||
|| **xmlId**
[`string`](../data-types.md) | External identifier ||
|| **commentsCount**
[`string`](../data-types.md) | Number of comments ||
|| **serviceCommentsCount**
[`string`](../data-types.md) | Number of system comments ||
|| **allowChangeDeadline**
[`string`](../data-types.md) | Allowed to change the deadline. Possible values: 
- `Y` — allowed
- `N` — not allowed ||
|| **allowTimeTracking**
[`string`](../data-types.md) | Allowed to track time. Possible values: 
- `Y` — allowed
- `N` — not allowed ||
|| **taskControl**
[`string`](../data-types.md) | Task control is enabled. Possible values: 
- `Y` — enabled
- `N` — disabled ||
|| **addInReport**
[`string`](../data-types.md) | Add to report. Possible values: 
- `Y` — add 
- `N` — do not add  ||
|| **forkedByTemplateId**
[`string`](../data-types.md) | Identifier of the template if the task was created from a template ||
|| **timeEstimate**
[`string`](../data-types.md) | Time estimate in seconds ||
|| **timeSpentInLogs**
[`string`](../data-types.md) | Actual time spent ||
|| **matchWorkTime**
[`string`](../data-types.md) | Consider working time. Possible values: 
- `Y` — yes
- `N` — no ||
|| **forumTopicId**
[`string`](../data-types.md) | Identifier of the comment topic ||
|| **forumId**
[`string`](../data-types.md) | Identifier of the forum ||
|| **siteId**
[`string`](../data-types.md) | Identifier of the site ||
|| **subordinate**
[`string`](../data-types.md) | Indicator that the task is a subtask. Possible values: 
- `Y` — yes
- `N` — no ||
|| **exchangeModified**
[`string`](../data-types.md) | Date of modification by synchronization in `ISO 8601` format ||
|| **exchangeId**
[`string`](../data-types.md) | Synchronization identifier ||
|| **outlookVersion**
[`string`](../data-types.md) | Version ||
|| **viewedDate**
[`string`](../data-types.md) | Date the task was viewed in `ISO 8601` format ||
|| **sorting**
[`string`](../data-types.md) | Sorting value ||
|| **durationFact**
[`string`](../data-types.md) | Actual duration ||
|| **isMuted**
[`string`](../data-types.md) | Indicator of "muted." Possible values: 
- `Y` — enabled
- `N` — disabled ||
|| **isPinned**
[`string`](../data-types.md) | Indicator of "pinned." Possible values: 
- `Y` — enabled
- `N` — disabled ||
|| **isPinnedInGroup**
[`string`](../data-types.md) | Indicator of "pinned in group." Possible values: 
- `Y` — pinned
- `N` — not pinned  ||
|| **flowId**
[`string`](../data-types.md) | Flow identifier ||
|| **descriptionInBbcode**
[`string`](../data-types.md) | Description in BBCode format. Possible values: 
- `Y` — enabled
- `N` — disabled  ||
|| **status**
[`string`](../data-types.md) | Task status. Possible values:
- `2` — waiting for execution
- `3` — in progress
- `4` — awaiting control
- `5` — completed
- `6` — postponed ||
|| **statusChangedDate**
[`string`](../data-types.md) | Date of status change in `ISO 8601` format ||
|| **durationPlan**
[`string`](../data-types.md) | Planned duration ||
|| **durationType**
[`string`](../data-types.md) | Unit of planned duration. Possible values: `secs`, `mins`, `hours`, `days`, `weeks`, `monts`, `years` ||
|| **favorite**
[`string`](../data-types.md) | Indicator of "favorite." Possible values: 
- `Y` — added to favorites
- `N` — not added to favorites ||
|| **groupId**
[`string`](../data-types.md) | Identifier of the group to which the task is linked ||
|| **auditors**
[`array`](../data-types.md) | List of user identifiers — task observers ||
|| **accomplices**
[`array`](../data-types.md) | List of user identifiers — Participants ||
|| **checklist**
[`object`](../data-types.md) | Object with checklist items. Key — item identifier, value — object with [item description](#checklist-item) ||
|| **group**
[`object`](../data-types.md) | Object with [group description](#group) ||
|| **creator**
[`object`](../data-types.md) | Object with [user description](#user) — Creator of the task ||
|| **responsible**
[`object`](../data-types.md) | Object with [user description](#user) — responsible for the task ||
|| **accomplicesData**
[`array`](../data-types.md) | Object with descriptions of users — Participants. 

The key of the object is the user identifier, and the value is the object with [user description](#user) ||
|| **auditorsData**
[`object`](../data-types.md) | Object with descriptions of users — task observers. 

The key of the object is the user identifier, and the value is the object with [user description](#user) ||
|| **newCommentsCount**
[`integer`](../data-types.md) | Number of new comments ||
|| **action**
[`object`](../data-types.md) | Object with [available actions](#action) on the task ||
|| **checkListTree**
[`object`](../data-types.md) | Object with [checklist tree description](#checklisttree) ||
|| **checkListCanAdd**
[`boolean`](../data-types.md) | Can add checklist items ||
|| **ufCrmTask**
[`array`](../data-types.md) | List of bindings to CRM entities in the format:
- `L_XX` — lead,
- `D_XX` — deal
- `C_XX` — contact
- `CO_XX` — company
- `SI_XX` — invoice
- `TXX_XX` — SPA | ||
|| **ufTaskWebdavFiles**
[`array`](../data-types.md) | List of files from Drive | ||
|| **ufMailMessage**
[`string`](../data-types.md) | e-mail | ||
|| **UF_\***
[`any`](../data-types.md) | Custom fields. 

More details in the article [{#T}](./user-field/index.md) | ||
|#

{% note info "" %}

To retrieve custom fields of the task, use the selection methods [tasks.task.get](./tasks-task-get.md) and [tasks.task.list](./tasks-task-list.md). Specify the required fields in the `SELECT` parameter.

System fields `UF_CRM_TASK`, `UF_TASK_WEBDAV_FILES`, and `UF_MAIL_MESSAGE` are not returned by default. Specify one of these fields in `SELECT` — all three will be returned. In the response, the fields are returned in camelCase: `ufCrmTask`, `ufTaskWebdavFiles`, `ufMailMessage`.

{% endnote %}

## Object checklist.item {#checklist-item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../data-types.md) | Item identifier ||
|| **taskId**
[`string`](../data-types.md) | Task identifier ||
|| **createdBy**
[`string`](../data-types.md) | Identifier of the item author ||
|| **parentId**
[`string`](../data-types.md) | Parent item. A value of `0` indicates a root item ||
|| **title**
[`string`](../data-types.md) | Item title ||
|| **sortIndex**
[`string`](../data-types.md) | Sorting index ||
|| **isComplete**
[`string`](../data-types.md) | Completion indicator. Possible values: 
- `Y` — item completed
- `N` — item not completed ||
|| **isImportant**
[`string`](../data-types.md) | Importance indicator. Possible values: 
- `Y` — important
- `N` — not important ||
|| **toggledBy**
[`string`](../data-types.md) | Identifier of the user who changed the item's status ||
|| **toggledDate**
[`string`](../data-types.md) | Date of the item's status change in `ISO 8601` format ||
|| **ufChecklistFiles**
[`boolean`](../data-types.md) | Indicator of the presence of files in the item ||
|| **members**
[`array`](../data-types.md) | List of item participants ||
|| **attachments**
[`array`](../data-types.md) | List of item attachments ||
|| **entityId**
[`string`](../data-types.md) | Task identifier ||
|| **action**
[`object`](../data-types.md) | Object with [available actions](#checklist-item-action) on the item ||
|#

### Object checklist.item.action {#checklist-item-action}

#|
|| **Name**
`type` | **Description** ||
|| **modify**
[`boolean`](../data-types.md) | Modification of the item is available ||
|| **remove**
[`boolean`](../data-types.md) | Removal of the item is available ||
|| **toggle**
[`boolean`](../data-types.md) | Toggling the item's status is available ||
|| **add**
[`boolean`](../data-types.md) | Adding a sub-item is available ||
|| **addAccomplice**
[`boolean`](../data-types.md) | Adding a Participant to the item is available ||
|#

## Object group {#group}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../data-types.md) | Group identifier ||
|| **name**
[`string`](../data-types.md) | Group name ||
|| **opened**
[`boolean`](../data-types.md) | Indicator of an open group ||
|| **membersCount**
[`integer`](../data-types.md) | Number of group members ||
|| **image**
[`string`](../data-types.md) | URL of the group image ||
|| **additionalData**
[`array`](../data-types.md) | Additional data of the group ||
|#

## Object with user description {#user}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../data-types.md) | User identifier ||
|| **name**
[`string`](../data-types.md) | User's first and last name ||
|| **link**
[`string`](../data-types.md) | Link to the user's profile ||
|| **icon**
[`string`](../data-types.md) | URL of the user's avatar ||
|| **workPosition**
[`string`](../data-types.md) | User's position ||
|#

## Object action {#action}

#|
|| **Name**
`type` | **Description** ||
|| **accept**
[`boolean`](../data-types.md) | Accepting the task is available. Deprecated field, no longer used ||
|| **decline**
[`boolean`](../data-types.md) | Declining the task is available. Deprecated field, no longer used ||
|| **complete**
[`boolean`](../data-types.md) | Completing the task is available ||
|| **approve**
[`boolean`](../data-types.md) | Approving the executor's work on the task is available when `taskControl` is enabled ||
|| **disapprove**
[`boolean`](../data-types.md) | Selecting what the executor needs to finish on the task is available when `taskControl` is enabled ||
|| **start**
[`boolean`](../data-types.md) | Starting execution is available ||
|| **pause**
[`boolean`](../data-types.md) | Pausing execution is available ||
|| **delegate**
[`boolean`](../data-types.md) | Delegation is available ||
|| **remove**
[`boolean`](../data-types.md) | Removal of the task is available ||
|| **edit**
[`boolean`](../data-types.md) | Editing is available ||
|| **defer**
[`boolean`](../data-types.md) | Deferring the task is available ||
|| **renew**
[`boolean`](../data-types.md) | Resuming is available ||
|| **create**
[`boolean`](../data-types.md) | Creating related tasks is available ||
|| **changeDeadline**
[`boolean`](../data-types.md) | Changing the deadline is available ||
|| **checklistAddItems**
[`boolean`](../data-types.md) | Adding checklist items is available ||
|| **addFavorite**
[`boolean`](../data-types.md) | Adding to favorites is available ||
|| **deleteFavorite**
[`boolean`](../data-types.md) | Removing from favorites is available ||
|| **rate**
[`boolean`](../data-types.md) | Rating is available ||
|| **take**
[`boolean`](../data-types.md) | Taking the task for work is available ||
|| **edit.originator**
[`boolean`](../data-types.md) | Editing the Creator is available ||
|| **checklist.reorder**
[`boolean`](../data-types.md) | Changing the order of checklist items is available ||
|| **elapsedtime.add**
[`boolean`](../data-types.md) | Adding time logs is available ||
|| **dayplan.timer.toggle**
[`boolean`](../data-types.md) | Managing the timer in the daily plan is available ||
|| **edit.plan**
[`boolean`](../data-types.md) | Editing planned parameters is available ||
|| **checklist.add**
[`boolean`](../data-types.md) | Adding a checklist is available ||
|| **favorite.add**
[`boolean`](../data-types.md) | Adding to favorites is available ||
|| **favorite.delete**
[`boolean`](../data-types.md) | Removing from favorites is available ||
|#

## Object checkListTree {#checklisttree}

#|
|| **Name**
`type` | **Description** ||
|| **nodeId**
[`integer`](../data-types.md) | Identifier of the root node of the checklist tree ||
|| **fields**
[`object`](../data-types.md) | Object with [description of node fields](#checklisttree-fields) ||
|| **action**
[`array`](../data-types.md) | Array of available actions for the node ||
|| **descendants**
[`array`](../data-types.md) | Array of child nodes ||
|#

### Object checkListTree.fields {#checklisttree-fields}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../data-types.md) | Identifier of the item for the root ||
|| **copiedId**
[`string`](../data-types.md) | Identifier of the source when copying ||
|| **entityId**
[`string`](../data-types.md) | Task identifier ||
|| **userId**
[`integer`](../data-types.md) | Identifier of the user forming the tree ||
|| **createdBy**
[`string`](../data-types.md) | Identifier of the item author ||
|| **parentId**
[`string`](../data-types.md) | Identifier of the parent ||
|| **title**
[`string`](../data-types.md) | Item title ||
|| **sortIndex**
[`string`](../data-types.md) | Sorting index ||
|| **displaySortIndex**
[`string`](../data-types.md) | Displayed sorting index ||
|| **isComplete**
[`boolean`](../data-types.md) | Indicator of the item's completion ||
|| **isImportant**
[`boolean`](../data-types.md) | Indicator of the item's importance ||
|| **completedCount**
[`integer`](../data-types.md) | Number of completed sub-items ||
|| **members**
[`array`](../data-types.md) | Array of item participants ||
|| **attachments**
[`array`](../data-types.md) | Array of item attachments ||
|| **nodeId**
[`string`](../data-types.md) | Identifier of the node ||
|#