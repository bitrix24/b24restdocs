# Task Template Fields v 3.0

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

This page describes the task template fields in REST 3.0. The [Template Object](#templatedto) section lists all template fields, and the other sections describe the fields of nested and related objects.

REST 3.0 methods are called at an address that contains `/rest/api/`. The names of the template methods match the names of the previous version methods, for example, `tasks.template.get`, but their parameter and response formats differ. The specifics of the new version are described in the [REST 3.0 overview](../../rest-v3.md).

## Which Fields the Methods Accept and Return {#methods}

#|
|| **Method** | **Accepts** | **Returns** ||
|| [tasks.template.add](./tasks-template-add-rest-v3.md) | Editable fields of the [template object](#templatedto) in `fields`. Required: `title`, `creatorId`, and the responsible person in `responsibleId` or `responsibleCollection` | The created template with all filled fields, including `tags`, `permissions`, and `replicateParams` ||
|| [tasks.template.update](./tasks-template-update-rest-v3.md) | Editable fields of the [template object](#templatedto) in `fields`, except `creatorId` and `tParamType` | Success flag `true` ||
|| [tasks.template.get](./tasks-template-get-rest-v3.md) | Template `id` and `select` | Without `select`, simple template fields without nested and related objects. The `tags`, `permissions`, `replicateParams`, `userFields`, `creator`, `group`, and `task` objects are returned only if they are specified in `select`. The method does not return the `parent`, `templateChildrenCount`, and `subTemplateIds` fields ||
|| [tasks.template.list](./tasks-template-list-rest-v3.md) | `select`, `filter`, `order`, `pagination` | Without `select`, only `id`. The `permissions`, `crmItemIds`, `checklist`, `requireResult`, `checklistAutonumbering`, `parent`, `group`, and `task` fields are not available in `select` ||
|| [tasks.template.copy](./tasks-template-copy-rest-v3.md) | `id` of the source template | A copy of the template with the same set of fields as `tasks.template.add` ||
|| [tasks.template.delete](./tasks-template-delete-rest-v3.md) | Template `id` | Success flag `true` ||
|#

Fields with the `null` value are not included in the response. For example, if a template has no parent template, the response does not contain the `baseTemplateId` field.

Boolean fields accept and return `true` or `false`. The string values `Y` and `N` used by the previous version methods are not accepted in REST 3.0: the method returns a validation error. The exception is `workdayOnly` in the [recurrence parameters object](#replicate-params).

The method rejects a field that cannot be changed with the "Field is read-only" error. Such fields are marked as read-only in the table.

## Template Object {#templatedto}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Template ID. Read-only. Available in filter and sorting ||
|| **title**
[`string`](../../data-types.md) | Title of the template and of the task created from it. Required for [creating a template](./tasks-template-add-rest-v3.md). Available in filter and sorting ||
|| **description**
[`string`](../../data-types.md) | Description of the template and of the task created from it, in BB code format ||
|| **descriptionInBbcode**
[`boolean`](../../data-types.md) | Indicates that the description is stored in BB code rather than HTML. Read-only ||
|| **creatorId**
[`integer`](../../data-types.md) | Creator ID. Required for [creating a template](./tasks-template-add-rest-v3.md), not available when updating a template.

Only an administrator can specify another employee as the creator. A user without administrator permissions specifies their own ID.

The user ID can be retrieved with the [user.get](../../user/user-get.md) method. Available in filter ||
|| **creator**
[`object`](../fields-rest-v3.md#user) | Creator. An object of type [user](../fields-rest-v3.md#user). Specify the object fields in `select` with a dot, for example, `creator.name`. Read-only ||
|| **responsibleId**
[`integer`](../../data-types.md) | ID of the responsible person for the task created from the template. If there are several responsible persons, the ID of the first one in the `responsibleCollection` list.

When writing, use `responsibleId` for one responsible person or `responsibleCollection` for several. If both fields are passed, `responsibleCollection` is used. Available in filter ||
|| **responsibleCollection**
[`array`](../../data-types.md) | List of responsible persons. Each element is a [member object](#member) with the `id` field, for example, `[{"id": 1269}]`.

Several responsible persons make sense together with `multitask: true` ||
|| **responsibleLastName**
[`string`](../../data-types.md) | Last name of the first responsible person. Read-only. Available in sorting ||
|| **accomplices**
[`array`](../../data-types.md) | List of participants. Each element is a [member object](#member) with the `id` field ||
|| **auditors**
[`array`](../../data-types.md) | List of auditors. Each element is a [member object](#member) with the `id` field ||
|| **groupId**
[`integer`](../../data-types.md) | ID of the project in which the task is created from the template. `0` means no project.

The ID can be retrieved with the [sonet_group.get](../../sonet-group/sonet-group-get.md) method. Available in filter and sorting ||
|| **group**
[`object`](../fields-rest-v3.md#group) | Project. An object of type [group](../fields-rest-v3.md#group). Specify the object fields in the `select` of [tasks.template.get](./tasks-template-get-rest-v3.md) with a dot, for example, `group.name`. Read-only ||
|| **taskId**
[`integer`](../../data-types.md) | ID of the task from which the template was created. `0` means the template was not created from a task. Available in filter and sorting ||
|| **task**
[`object`](#task) | Task from which the template was created. A [task object](#task) with the `id` and `title` fields. Specify the object fields in the `select` of [tasks.template.get](./tasks-template-get-rest-v3.md) with a dot, for example, `task.title`. Read-only ||
|| **parentId**
[`integer`](../../data-types.md) | ID of the task whose subtask the task created from the template becomes ||
|| **parent**
[`object`](../../data-types.md) | Not filled. The parent task is set by `parentId`, the parent template by `baseTemplateId` ||
|| **baseTemplateId**
[`integer`](../../data-types.md) | ID of the parent template to which this template is linked as a child. Available in filter and sorting ||
|| **templateChildrenCount**
[`integer`](../../data-types.md) | Number of child templates. Returned only by [tasks.template.list](./tasks-template-list-rest-v3.md). Read-only. Available in sorting ||
|| **subTemplateIds**
[`array`](../../data-types.md) | IDs of the child templates available to the user. Returned only by [tasks.template.list](./tasks-template-list-rest-v3.md). Read-only. Links are changed by the [tasks.template.relation.child.*](./relation/tasks-template-relation-child-list-rest-v3.md) methods ||
|| **stageId**
[`integer`](../../data-types.md) | ID of the stage the created task goes to. Read-only. Available in filter and sorting ||
|| **xmlId**
[`string`](../../data-types.md) | External code of the template. Read-only. Available in filter ||
|| **status**
[`string`](../../data-types.md) | Code of the status with which the task is created from the template, for example, `1`. Read-only ||
|| **addInReport**
[`boolean`](../../data-types.md) | Indicates that the created task is included in reports. Read-only ||
|| **priority**
[`string`](../../data-types.md) | Priority of the created task. Possible values:
- `low` — low
- `average` — medium
- `high` — high

Default: `average`. The method saves a value outside the list as `average`. Available in filter ||
|| **tParamType**
[`string`](../../data-types.md) | Template type. Possible values:
- `usual` — regular template
- `new_users` — template for a new employee

Default: `usual`. The method saves a value outside the list as `usual`. Set only when creating a template. Available in filter and sorting ||
|| **deadlineAfterTs**
[`integer`](../../data-types.md) | Number of seconds after the task is created when its deadline occurs, for example, `86400` — one day later. `0` means no deadline ||
|| **startDatePlanTs**
[`integer`](../../data-types.md) | Number of seconds after the task is created when its planned start occurs ||
|| **endDatePlanTs**
[`integer`](../../data-types.md) | Number of seconds after the task is created when its planned end occurs ||
|| **allowsChangeDeadline**
[`boolean`](../../data-types.md) | The responsible person can change the deadline of the created task. Default: `false` ||
|| **allowsTimeTracking**
[`boolean`](../../data-types.md) | Time tracking is enabled in the created task. Default: `false` ||
|| **matchesWorkTime**
[`boolean`](../../data-types.md) | The dates of the created task are calculated based on the company's working hours, excluding weekends and holidays. Default: `false` ||
|| **needsControl**
[`boolean`](../../data-types.md) | The creator reviews the completed task. Default: `false` ||
|| **requireResult**
[`boolean`](../../data-types.md) | The created task cannot be completed until a result is added. If the parameter has not been set, the field is not in the response ||
|| **checklistAutonumbering**
[`boolean`](../../data-types.md) | Checklist items and subitems of the template and of the tasks created from it are numbered automatically. If the parameter has not been set, the field is not in the response ||
|| **estimatedTime**
[`integer`](../../data-types.md) | Estimated effort of the created task in seconds ||
|| **multitask**
[`boolean`](../../data-types.md) | Each responsible person from `responsibleCollection` receives a separate task from the template. Default: `false` ||
|| **siteId**
[`string`](../../data-types.md) | ID of the site to which the template is bound. Default: `s1` ||
|| **replicate**
[`boolean`](../../data-types.md) | Tasks are created from the template automatically, on the schedule from `replicateParams`. To enable recurrence, pass `replicateParams` with `time` filled in. Default: `false`. Available in filter ||
|| **replicateParams**
[`object`](#replicate-params) | Schedule on which tasks are created. [Recurrence parameters object](#replicate-params) ||
|| **tParamReplicationCount**
[`integer`](../../data-types.md) | Number of tasks already created on the schedule. Taken into account if `replicateParams.repeatTill` equals `times` ||
|| **dependsOn**
[`array`](../../data-types.md) | IDs of the tasks on which the created task depends. No more than 50 positive integers. The user must have read access to each task.

When the template is updated, links to tasks that the user cannot see are kept. Available in sorting ||
|| **fileIds**
[`array`](../../data-types.md) | IDs of the files attached to the template. Template files are returned by the [tasks.template.file.list](./tasks-template-file-list-rest-v3.md) method ||
|| **crmItemIds**
[`array`](../../data-types.md) | IDs of CRM items linked to the created task, with a type prefix, for example, `D_27` for a deal ||
|| **checklist**
[`array`](../../data-types.md) | IDs of checklist items. Read-only. Items are changed by the [tasks.template.checklist.*](./checklist/tasks-template-checklist-get-rest-v3.md) methods ||
|| **tags**
[`array`](#tags) | Tags of the created task. Each element is a [tag object](#tags). When writing, only the `name` field is taken into account. Available in filter by tag name ||
|| **permissions**
[`array`](#permissions) | Template access permissions. Each element is an [access permission object](#permissions) ||
|| **userFields**
[`array`](#user-fields) | Custom field values. Each element is a [custom field object](#user-fields) ||
|#

## Member Object {#member}

An element of the `responsibleCollection`, `accomplices`, and `auditors` arrays.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | User ID. Can be retrieved with the [user.get](../../user/user-get.md) method ||
|#

If the template is recurring, the creator must have permission to assign tasks to each responsible person and participant, and, when the project changes, permission to create tasks in the project. Otherwise, the method returns an access error.

## Recurrence Parameters Object {#replicate-params}

The `replicateParams` object is replaced entirely. Parameters that are not passed receive default values. Numeric parameters are passed and returned as strings, for example, `"1"`.

#|
|| **Name**
`type` | **Description** ||
|| **period**
[`string`](../../data-types.md) | How often to create a task. Possible values:
- `daily` — every day
- `weekly` — every week
- `monthly` — every month
- `yearly` — every year ||
|| **time**
[`string`](../../data-types.md) | Time of task creation in the `HH:MM` format, for example, `09:00`. Without this parameter, recurrence is not saved ||
|| **timezoneOffset**
[`string`](../../data-types.md) | Timezone offset in seconds used to calculate the schedule. Default: `0` ||
|| **startDate**
[`string`](../../data-types.md) | Start date of recurrence in the Bitrix24 date format, for example, `12/01/2026` for the US format or `01.12.2026` for the European format. The method does not accept the ISO 8601 format ||
|| **repeatTill**
[`string`](../../data-types.md) | When to stop recurrence. Possible values:
- `endless` — no limit
- `times` — after the number of tasks from `times`
- `date` — after the date from `endDate` ||
|| **endDate**
[`string`](../../data-types.md) | End date of recurrence in the Bitrix24 date format. Used if `repeatTill` equals `date` ||
|| **times**
[`string`](../../data-types.md) | Number of tasks to create. Used if `repeatTill` equals `times` ||
|| **everyDay**
[`string`](../../data-types.md) | Interval in days between tasks when `period` equals `daily`. Default: `1` ||
|| **workdayOnly**
[`string`](../../data-types.md) | Create the task on working days only. Possible values:
- `Y` — working days only
- `N` — any day

Default: `N` ||
|| **dailyMonthInterval**
[`string`](../../data-types.md) | When `period` equals `daily`, adds 30 days per month to the interval in days ||
|| **everyWeek**
[`string`](../../data-types.md) | Interval in weeks between tasks when `period` equals `weekly`. Default: `1` ||
|| **weekDays**
[`array`](../../data-types.md) | Days of the week on which to create the task when `period` equals `weekly`. Numbers from `1` — Monday to `7` — Sunday ||
|| **monthlyType**
[`string`](../../data-types.md) | How to choose the day of the month when `period` equals `monthly`. Possible values:
- `1` — by day of the month
- `2` — by day of the week

The method rejects any other value with a validation error ||
|| **monthlyDayNum**
[`string`](../../data-types.md) | Day of the month when `monthlyType` equals `1` ||
|| **monthlyMonthNum1**
[`string`](../../data-types.md) | Interval in months between tasks when `monthlyType` equals `1` ||
|| **monthlyWeekDayNum**
[`string`](../../data-types.md) | Week number in the month when `monthlyType` equals `2`: from `0` — first to `4` — last ||
|| **monthlyWeekDay**
[`string`](../../data-types.md) | Day of the week when `monthlyType` equals `2`: from `0` — Monday to `6` — Sunday ||
|| **monthlyMonthNum2**
[`string`](../../data-types.md) | Interval in months between tasks when `monthlyType` equals `2` ||
|| **yearlyType**
[`string`](../../data-types.md) | How to choose the day of the year when `period` equals `yearly`. Possible values:
- `1` — by day of the month
- `2` — by day of the week

The method rejects any other value with a validation error ||
|| **yearlyDayNum**
[`string`](../../data-types.md) | Day of the month when `yearlyType` equals `1` ||
|| **yearlyMonth1**
[`string`](../../data-types.md) | Month when `yearlyType` equals `1`: from `0` — January to `11` — December ||
|| **yearlyWeekDayNum**
[`string`](../../data-types.md) | Week number in the month when `yearlyType` equals `2`: from `0` — first to `4` — last ||
|| **yearlyWeekDay**
[`string`](../../data-types.md) | Day of the week when `yearlyType` equals `2`: from `0` — Monday to `6` — Sunday ||
|| **yearlyMonth2**
[`string`](../../data-types.md) | Month when `yearlyType` equals `2`: from `0` — January to `11` — December ||
|| **deadlineOffset**
[`integer`](../../data-types.md) | Number of seconds after the task is created when its deadline occurs. Matches the template's `deadlineAfterTs` ||
|| **nextExecutionTime**
[`string`](../../data-types.md) | Date and time of the next task creation in the Bitrix24 date format. Calculated automatically, read-only ||
|#

## Access Permission Object {#permissions}

An element of the `permissions` array. When writing, the list of permissions is replaced entirely. When a template is created, the creator receives the `full` permission automatically.

Permissions can be changed by an administrator, the template author, or a user with the `full` permission. Permission configuration is not available on all plans: on a plan without this feature, the method returns an error, so remove `permissions` from the request.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | ID of the access permission record. Read-only ||
|| **accessCode***
[`string`](../../data-types.md) | Who is granted the permission. Possible values:
- `U<id>` — employee, for example, `U1`
- `D<id>` — department
- `DR<id>` — department with all subdepartments
- `G<id>` — user group
- `UA` — all employees ||
|| **permission***
[`string`](../../data-types.md) | Access type. Possible values:
- `read_only` — read only
- `full` — read, edit, and delete ||
|#

## Tag Object {#tags}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Tag ID. Read-only ||
|| **name**
[`string`](../../data-types.md) | Tag name ||
|| **ownerId**
[`integer`](../../data-types.md) | ID of the user who owns the tag. Read-only ||
|#

## Custom Field Object {#user-fields}

Task custom fields are created by the [task.item.userfield.*](../user-field/index.md) methods.

#|
|| **Name**
`type` | **Description** ||
|| **key**
[`string`](../../data-types.md) | Field code, for example, `UF_AUTO_615763798639` ||
|| **value**
[`any`](../../data-types.md) | Field value. The type depends on the field settings. If the field is empty, `null` ||
|#

## Task Object {#task}

Task from which the template was created. Only these fields are available in the `select` of the [tasks.template.get](./tasks-template-get-rest-v3.md) method.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Task ID ||
|| **title**
[`string`](../../data-types.md) | Task title. If the user has no access to the task, the object is not in the response ||
|#

## Continue Learning

- [{#T}](./tasks-template-get-rest-v3.md)
- [{#T}](./tasks-template-list-rest-v3.md)
- [{#T}](./tasks-template-add-rest-v3.md)
- [{#T}](./tasks-template-update-rest-v3.md)
- [{#T}](./fields.md)
- [{#T}](../../rest-v3.md)
