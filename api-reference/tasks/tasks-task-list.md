# Get the list of tasks tasks.task.list

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- curl examples are missing
- response in case of error is absent
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.list` returns an array of tasks, each containing an array of fields. Unlike [task.item.list](./deprecated/task-item/task-item-list.md), parameters in the `tasks.task.list` request can be specified in any order, and unnecessary parameters can be omitted.

To retrieve data for all tasks, the user must have admin rights. A department head will only have access to tasks within their branch of the hierarchy.

Tasks marked as "Favorite" can also be retrieved by setting the filter parameter `$filter[::SUBFILTER-PARAMS][FAVORITE]=Y`.

{% note warning %}

It is necessary to specify fields in `select`, as default fields may change in the future.

{% endnote %}

#|
|| **Parameter** / **Type** | **Description** ||
|| **order**
[`unknown`](../data-types.md) | An array for sorting the result. The array format is `{"sorting_field": 'sorting_direction' [, ...]}`.
The sorting field can take the following values: 
- **ID** — Task identifier.
- **TITLE** — Task title.
- **TIME_SPENT_IN_LOGS** — Time spent recorded in the change history.
- **DATE_START** — Task start date.
- **CREATED_DATE** — Task creation date.
- **CHANGED_DATE** — Date of the last task modification.
- **CLOSED_DATE** — Task completion date.
- **START_DATE_PLAN** — Planned start date for task execution.
- **END_DATE_PLAN** — Planned completion date for task execution.
- **DEADLINE** — Task deadline.
- **REAL_STATUS** — Task status. Constants reflecting task statuses: 
    - STATE_NEW = 1;
    - STATE_PENDING = 2;
    - STATE_IN_PROGRESS = 3;
    - STATE_SUPPOSEDLY_COMPLETED = 4;
    - STATE_COMPLETED = 5;
    - STATE_DEFERRED = 6;
    - STATE_DECLINED = 7;
- **STATUS_COMPLETE** — Task completion flag.
- **PRIORITY** — Task priority.
- **MARK** — Rating for task completion.
- **CREATED_BY_LAST_NAME** — Last name of the task creator.
- **RESPONSIBLE_LAST_NAME** — Last name of the task assignee.
- **GROUP_ID** — Identifier of the workgroup.
- **TIME_ESTIMATE** — Time allocated for the task.
- **ALLOW_CHANGE_DEADLINE** — Flag allowing the assignee to change the deadline.
- **ALLOW_TIME_TRACKING** — Flag enabling time tracking for the task.
- **MATCH_WORK_TIME** — Flag indicating the need to skip weekends.
- **FAVORITE** — Flag indicating that the task has been added to favorites.
- **SORTING** — Sorting index.
- **MESSAGE_ID** — Identifier of the search index.

{% note info %}

In the on-premise version, the list of fields for sorting can be obtained using the method `CTasks::getAvailableOrderFields()`.

{% endnote %}

The sorting direction can take the following values: 

- **asc** — ascending;
- **desc** — descending;

Optional. By default, it is sorted in descending order by task identifier. 
 ||
|| **filter**
[`unknown`](../data-types.md) | An array in the format `{"filter_field": "filter_value" [, ...]}`. The filter field can take the following values:
- **ID** - task identifier;
- **PARENT_ID** - parent task identifier;
- **GROUP_ID** - workgroup identifier;
- **CREATED_BY** - creator;
- **STATUS_CHANGED_BY** - user who last changed the task status;
- **PRIORITY** - priority;
- **FORUM_TOPIC_ID** - forum topic identifier;
- **RESPONSIBLE_ID** - assignee;
- **TITLE** - task title (can be searched using the pattern [\%_]);
- **REAL_STATUS** - task status. Corresponds to the `status` field in the response. Constants reflecting task statuses:
    - STATE_NEW = 1;
    - STATE_PENDING = 2;
    - STATE_IN_PROGRESS = 3;
    - STATE_SUPPOSEDLY_COMPLETED = 4;
    - STATE_COMPLETED = 5;
    - STATE_DEFERRED = 6;
    - STATE_DECLINED = 7;
- **STATUS** - status for sorting. Corresponds to the `subStatus` field in the response. Similar to **REAL_STATUS**, but has three additional meta-statuses:
    - **-3** - task is almost overdue;
    - **-2** - unviewed task;
    - **-1** - overdue task.
- **MARK** - rating;
- **SITE_ID** - site identifier;
- **ADD_IN_REPORT** - task in report (Y\|N);
- **DATE_START** - start date;
- **DEADLINE** - deadline;
- **CREATED_DATE** - creation date;
- **CLOSED_DATE** - completion date;
- **CHANGED_DATE** - last modification date;
- **ACCOMPLICE** - identifier of the participant;
- **AUDITOR** - identifier of the auditor;
- **DEPENDS_ON** - identifier of the previous task;
- **ONLY_ROOT_TASKS** - only tasks that are not subtasks (root tasks), as well as subtasks of the parent task to which the current user does not have access (Y\|N).
- **STAGE_ID** - stage;
- **UF_CRM_TASK** - CRM entities;
- **TAG** - Tasks Tags;

Before the filter field name, a filtering type can be specified:
- "!" - not equal
- "<" - less than
- "<=" - less than or equal to
- ">" - greater than
- ">=" - greater than or equal to 

*"filter values"* - a single value or an array.

Optional. By default, records are not filtered. ||
|| **select**
[`unknown`](../data-types.md) | An array of record fields that will be returned by the method. Only the necessary fields can be specified.

Available fields: 
- **ID** - task identifier;
- **PARENT_ID** - parent task identifier;
- **TITLE** - task title;
- **DESCRIPTION** - description;
- **MARK** - rating;
- **PRIORITY** - priority:
    - **0** - low;
    - **1** - medium;
    - **2** - high.
- **STATUS** - status. Returns the regular status `status` and meta-status `subStatus`;
- **MULTITASK** - multiple task;
- **NOT_VIEWED** - unviewed task;
- **REPLICATE** - recurring task;
- **GROUP_ID** - workgroup;
- **STAGE_ID** - stage;
- **CREATED_BY** - creator;
- **CREATED_DATE** - creation date;
- **RESPONSIBLE_ID** - assignee;
- **ACCOMPLICES** - identifier of the participant;
- **AUDITORS** - identifier of the auditor;
- **CHANGED_BY** - who modified the task;
- **CHANGED_DATE** - modification date;
- **STATUS_CHANGED_DATE** - status change date;
- **CLOSED_BY** - who closed the task;
- **CLOSED_DATE** - task closure date;
- **DATE_START** - start date;
- **DEADLINE** - deadline;
- **START_DATE_PLAN** - planned start;
- **END_DATE_PLAN** - planned completion;
- **GUID** - GUID (statistically unique 128-bit identifier);
- **XML_ID** - external code;
- **COMMENTS_COUNT** - number of comments;
- **NEW_COMMENTS_COUNT** - number of new comments;
- **TASK_CONTROL** - accept for work;
- **ADD_IN_REPORT** - add to report;
- **FORKED_BY_TEMPLATE_ID** - created from a template;
- **TIME_ESTIMATE** - time allocated for the task;
- **TIME_SPENT_IN_LOGS** - time spent from the change history;
- **MATCH_WORK_TIME** - skip weekends;
- **FORUM_TOPIC_ID** - forum topic identifier;
- **FORUM_ID** - forum identifier;
- **SITE_ID** - site identifier;
- **SUBORDINATE** - subordinate task;
- **FAVORITE** - Favorite;
- **VIEWED_DATE** - date of last view;
- **SORTING** - sorting index;
- **DURATION_PLAN** - time spent (planned);
- **DURATION_FACT** - time spent (actual);
- **DURATION_TYPE** - unit type in planned duration: days, hours, or minutes.

By default, all **non-computed** fields of the main query table will be returned.

The list of fields can be clarified by sending a request to [tasks.task.getFields](tasks-task-get-fields.md). ||
|| **limit**
[`unknown`](../data-types.md) | Number of records. This parameter is specified if you need to retrieve a number of records greater than the default value (50). It is not possible to return all records in one request; this is a limitation of all REST API methods. You can retrieve all leads in several requests of 50 records each. To do this, simply pass the parameter start with a value that is a multiple of 50. Example: 
```js
start=0
start=50
start=100
```
||
|| **start**
[`unknown`](../data-types.md) | How many initial records to skip in the result. Due to technical limitations, the value of this parameter must always be a multiple of 50. For example, with a value of 50, the 51st record and subsequent ones will be displayed, while the first 50 records will be skipped.

With a value of `-1`, the count will be disabled. 

Works for https requests. Example:
```js
BX24.callMethod('tasks.task.list',{start: 1150})
```
||
|#

## Example 1

Output all unique tasks added to "Favorites" with a status greater than 2:

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'tasks.task.list',
        {
            filter:{
                '>STATUS':2,
                'REPLICATE':'N',
                '::SUBFILTER-PARAMS':{
                    FAVORITE:'Y'
                }
            }
        },
        function(res){console.log(res.answer.result);}
    );
    ```

{% endlist %}

## Response in case of success

> 200 OK

```js
{
    "result": {
        "tasks": [
            {
                "id": "434",
                "parentId": "0",
                "title": "test task 1",
                "description": "",
                "mark": null,
                "priority": "1",
                "multitask": "N",
                "notViewed": "N",
                "replicate": "N",
                "stageId": "0",
                "createdBy": "1",
                "createdDate": "2024-11-22T13:58:17+02:00",
                "responsibleId": "1",
                "changedBy": "1",
                "changedDate": "2024-11-26T13:43:28+02:00",
                "statusChangedBy": "1",
                "closedBy": "0",
                "closedDate": null,
                "activityDate": "2024-11-22T13:58:17+02:00",
                "dateStart": "2024-11-26T13:43:28+02:00",
                "deadline": null,
                "startDatePlan": null,
                "endDatePlan": null,
                "guid": "{8a261464-64eb-4d04-827b-37ded5433e85}",
                "xmlId": null,
                "commentsCount": null,
                "serviceCommentsCount": null,
                "allowChangeDeadline": "Y",
                "allowTimeTracking": "N",
                "taskControl": "Y",
                "addInReport": "N",
                "forkedByTemplateId": null,
                "timeEstimate": "0",
                "timeSpentInLogs": null,
                "matchWorkTime": "N",
                "forumTopicId": null,
                "forumId": null,
                "siteId": "s1",
                "subordinate": "N",
                "exchangeModified": null,
                "exchangeId": null,
                "outlookVersion": "2",
                "viewedDate": "2024-11-26T13:40:45+02:00",
                "sorting": null,
                "durationFact": null,
                "isMuted": "N",
                "isPinned": "N",
                "isPinnedInGroup": "N",
                "flowId": null,
                "descriptionInBbcode": "Y",
                "status": "3",
                "statusChangedDate": "2024-11-26T13:43:28+02:00",
                "durationPlan": null,
                "durationType": "days",
                "favorite": "N",
                "groupId": "0",
                "auditors": [],
                "accomplices": [],
                "newCommentsCount": 0,
                "group": [],
                "creator": {
                    "id": "1",
                    "name": "Admin Adminov",
                    "link": "/company/personal/user/1/",
                    "icon": "/bitrix/images/tasks/default_avatar.png",
                    "workPosition": "Chief"
                },
                "responsible": {
                    "id": "1",
                    "name": "Admin Adminov",
                    "link": "/company/personal/user/1/",
                    "icon": "/bitrix/images/tasks/default_avatar.png",
                    "workPosition": "Chief"
                },
                "accomplicesData": [],
                "auditorsData": [],
                "subStatus": "3"
            }
        ]
    },
    "time": {
        "start": 1552382093.81029,
        "finish": 1552382093.927268,
        "duration": 0.11697793006896973,
        "processing": 0.018744230270385742,
        "date_start": "2019-03-12T11:14:53+02:00",
        "date_finish": "2019-03-12T11:14:53+02:00"
    }
}
```

## Example 2

Output all tasks with the title "task for test", filtering by fields `ID`, `TITLE`, `STATUS`, sorting by the field `ID` (ascending order):

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'tasks.task.list',
        {filter:{TITLE:'task for test'}, select: ['ID','TITLE','STATUS'], order:{ID:'asc'}},
        function(res){console.log(res.answer.result);}
    );
    ```

{% endlist %}

## Example 3

Example of disabling pagination:

{% list tabs %}

- PHP

    ```php
    $result = CRest::call(
        'tasks.task.list',
        [
            'filter' => [
                '>ID' => 50
            ],
            'start' => -1,
        ]
    );
    ```

{% endlist %}

## Example 4

### How to get a filtered list of tasks via an HTTP request?

Task filters by ID, date, status. For the filter `'=ID' => 3`, it is recommended to use [tasks.task.get](.) as it does not have pagination.

{% list tabs %}

- PHP

    ```php
    $filter = [];
    //by id
    $filter = [
        '>ID' => 3
    ];
    $filter = [
        '=ID' => 3//recommend: CRest::call('tasks.task.get');
    ];
    //by date
    $filter = [
        '<CREATED_DATE' => date(DATE_ATOM, mktime(12, 22, 37, 7, 25, 2019))
    ];
    //by status
    $filter = [
        '>STATUS' => 2 // 2 is enum value. for current client: CRest::call( 'tasks.task.getFields');
    ];
    $result = CRest::call(
        'tasks.task.list',
        [
            'filter' => $filter,
            'select' => [
                'ID',
                'TITLE',
                'CREATED_DATE'
            ]
        ]
    );
    //all fields
    $fields = CRest::call( 'tasks.task.getFields');
    echo '<pre>';
    print_r([$filter, $result, $fields]);
    echo '</pre>';
    $result = CRest::call(
        'tasks.task.get',
        [
            'taskId' => 3,
            'select' => [
                'ID',
                'TITLE',
                'CREATED_DATE'
            ]
        ]
    );
    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

{% include [Footnote on examples](../../_includes/examples.md) %}

## Continue your study

- [{#T}](../../tutorials/tasks/how-to-create-comment-with-file.md)
- [{#T}](../../tutorials/tasks/how-to-upload-file-to-task.md)
- [{#T}](../../tutorials/tasks/how-to-create-task-with-file.md)
