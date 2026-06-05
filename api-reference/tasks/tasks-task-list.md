# Get Task List tasks.task.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.list` retrieves a list of tasks with pagination.

Access to data depends on permissions:
- Administrators can see all tasks,
- Managers can see their employees' tasks,
- Others can only see tasks available to them.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **order**
[`object`](../data-types.md) | An object for sorting the task list in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

The sorting direction can take the following values:

- `asc` — ascending
- `desc` — descending

By default, it sorts in descending order by `ID`.

The field for sorting can take the following values:
- `ID` — task identifier  
- `TITLE` — task title  
- `TIME_SPENT_IN_LOGS` — time spent recorded in the change history  
- `DATE_START` — task start date  
- `CREATED_DATE` — task creation date  
- `CHANGED_DATE` — last task modification date  
- `CLOSED_DATE` — task completion date  
- `ACTIVITY_DATE` — date of last activity  
- `START_DATE_PLAN` — planned start date for task execution  
- `END_DATE_PLAN` — planned completion date for task execution  
- `DEADLINE` — task deadline  
- `REAL_STATUS` — task status  
- `STATUS_COMPLETE` — task completion flag  
- `PRIORITY` — task priority  
- `MARK` — task performance rating  
- `CREATED_BY_LAST_NAME` — last name of the Creator  
- `RESPONSIBLE_LAST_NAME` — last name of the Participant  
- `GROUP_ID` — identifier of the working group  
- `TIME_ESTIMATE` — time allocated for the task  
- `ALLOW_CHANGE_DEADLINE` — flag allowing the Participant to change the deadline  
- `ALLOW_TIME_TRACKING` — flag enabling time tracking for the task  
- `MATCH_WORK_TIME` — flag indicating the need to skip weekends  
- `FAVORITE` — flag indicating that the task has been added to favorites  
- `SORTING` — sorting index  
- `IS_PINNED` — pinned status  
- `IS_PINNED_IN_GROUP` — pinned in group status

||
|| **filter**
[`object`](../data-types.md) | An object for filtering the task list in the format `{"field_1": "value_1", ... "field_N": "value_N"}`. The filterable fields can take the following values:

- `ID` — task identifier  
- `PARENT_ID` — parent task identifier  
- `GROUP_ID` — working group identifier  
- `CREATED_BY` — Creator  
- `STATUS_CHANGED_BY` — user who last changed the task status  
- `PRIORITY` — priority  
- `FORUM_TOPIC_ID` — forum topic identifier  
- `RESPONSIBLE_ID` — Participant  
- `TITLE` — task title (can search by pattern [\%_])  
- `TAG` — task tag  
- `REAL_STATUS` — task status. Corresponds to the `status` field in the response.
    - `2` — waiting for execution
    - `3` — in progress
    - `4` — awaiting control
    - `5` — completed
    - `6` — postponed  
- `STATUS` — status for sorting. Corresponds to the `subStatus` field in the response. Similar to `REAL_STATUS`, but has three additional meta-statuses:
    - `-3` — task is almost overdue
    - `-2` — unviewed task
    - `-1` — overdue task
- `MARK` — rating  
- `SITE_ID` — site identifier  
- `ADD_IN_REPORT` — task in report  
- `DATE_START` — start date  
- `DEADLINE` — deadline  
- `CREATED_DATE` — creation date  
- `CLOSED_DATE` — completion date  
- `CHANGED_DATE` — last modification date  
- `ACCOMPLICE` — identifier of the Participant  
- `AUDITOR` — identifier of the auditor  
- `DEPENDS_ON` — identifier of the previous task  
- `ONLY_ROOT_TASKS` — only root tasks and subtasks without access to the parent  
- `STAGE_ID` — stage  
- `SPRINT_ID` — sprint identifier  
- `BACKLOG_ID` — backlog identifier  
- `UF_CRM_TASK` — binding to CRM entities

To get tasks from Favorites, add the filter parameter `$filter[::SUBFILTER-PARAMS][FAVORITE]=Y`.

You can specify the type of filtering before the name of the filterable field:
- `!` — not equal
- `<` — less than
- `<=` — less than or equal to
- `>` — greater than
- `>=` — greater than or equal to

By default, records are not filtered ||
|| **select**
[`array`](../data-types.md) | An array containing a [list of fields](./fields.md) to select. 

By default, the system returns only those fields stored in the record — without additional data calculated on the fly.

{% note warning %}

Always specify fields in `select`. The default set of fields may change.

{% endnote %}
||
|| **params**
[`object`](../data-types.md) | Additional information that can be retrieved about the task:
- `WITH_RESULT_INFO` — information about the result in the task
- `WITH_TIMER_INFO` — data on time spent
- `WITH_PARSED_DESCRIPTION` — description with HTML markup
||
|| **start**
[`integer`](../data-types.md) | This parameter is used to manage pagination.

The page size is always static — 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N - 1) * 50`, where `N` — the desired page number ||
|#

## Code Examples

{% include [Examples Note](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"DEADLINE":"asc","PRIORITY":"desc"},"filter":{"!STATUS":6,">=DEADLINE":"'"$(date +%Y-%m-%d)"'","RESPONSIBLE_ID":547,"::SUBFILTER-PARAMS":{"FAVORITE":"Y"}},"select":["ID","TITLE","DESCRIPTION","STATUS","subStatus","DEADLINE","CREATED_DATE","RESPONSIBLE_ID","ACCOMPLICES","AUDITORS","TAGS","COUNTERS","PRIORITY","MARK"],"params":{"WITH_TIMER_INFO":true,"WITH_RESULT_INFO":true,"WITH_PARSED_DESCRIPTION":true}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"DEADLINE":"asc","PRIORITY":"desc"},"filter":{"!STATUS":6,">=DEADLINE":"'"$(date +%Y-%m-%d)"'","RESPONSIBLE_ID":547,"::SUBFILTER-PARAMS":{"FAVORITE":"Y"}},"select":["ID","TITLE","DESCRIPTION","STATUS","subStatus","DEADLINE","CREATED_DATE","RESPONSIBLE_ID","ACCOMPLICES","AUDITORS","TAGS","COUNTERS","PRIORITY","MARK"],"params":{"WITH_TIMER_INFO":true,"WITH_RESULT_INFO":true,"WITH_PARSED_DESCRIPTION":true},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of a single task in result.tasks. Only the fields read below are typed here;
    // the request selects more — add them to this type as you start using them.
    type TaskListItem = {
      id: string
      title: string
      status: string
      deadline: ISODate | null
      responsibleId: string
    }

    try {
      // tasks.task.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<{ tasks: TaskListItem[] }>({
        method: 'tasks.task.list',
        params: {
          // Sorting: nearest deadline first, then higher priority
          order: { DEADLINE: 'asc', PRIORITY: 'desc' },
          filter: {
            '!STATUS': 6, // exclude deferred tasks
            '>=DEADLINE': new Date().toISOString().split('T')[0], // not overdue (UTC date; adjust to the account time zone if needed)
            RESPONSIBLE_ID: 547, // tasks of a specific responsible person
            '::SUBFILTER-PARAMS': { FAVORITE: 'Y' } // favorites only
          },
          // Request only the fields you need
          select: [
            'ID', 'TITLE', 'DESCRIPTION', 'STATUS', 'subStatus',
            'DEADLINE', 'CREATED_DATE', 'RESPONSIBLE_ID',
            'ACCOMPLICES', 'AUDITORS', 'TAGS', 'COUNTERS',
            'PRIORITY', 'MARK'
          ],
          // Extra computed data
          params: {
            WITH_TIMER_INFO: true,
            WITH_RESULT_INFO: true,
            WITH_PARSED_DESCRIPTION: true
          },
          // Page offset (not the page number): the page size is fixed at 50, so the
          // Nth page is start = (N - 1) * 50.
          start: 0
        },
        requestId: Text.getUuidRfc4122() // optional unique tracking id for this request
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const tasks = response.getData()!.result.tasks
        // getTotal() is deprecated (removed in SDK 2.0); for the full match count fetch
        // everything via a list helper (see above) and read its length.
        console.info(`Loaded ${tasks.length} tasks (one page)`)
        for (const task of tasks) {
          console.info(`#${task.id}: ${task.title}`)
        }
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function listTasks() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // tasks.task.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'tasks.task.list',
            params: {
              // Sorting: nearest deadline first, then higher priority
              order: { DEADLINE: 'asc', PRIORITY: 'desc' },
              filter: {
                '!STATUS': 6, // exclude deferred tasks
                '>=DEADLINE': new Date().toISOString().split('T')[0], // not overdue (UTC date; adjust to the account time zone if needed)
                RESPONSIBLE_ID: 547, // tasks of a specific responsible person
                '::SUBFILTER-PARAMS': { FAVORITE: 'Y' } // favorites only
              },
              // Request only the fields you need
              select: [
                'ID', 'TITLE', 'DESCRIPTION', 'STATUS', 'subStatus',
                'DEADLINE', 'CREATED_DATE', 'RESPONSIBLE_ID',
                'ACCOMPLICES', 'AUDITORS', 'TAGS', 'COUNTERS',
                'PRIORITY', 'MARK'
              ],
              // Extra computed data
              params: {
                WITH_TIMER_INFO: true,
                WITH_RESULT_INFO: true,
                WITH_PARSED_DESCRIPTION: true
              },
              // Page offset (not the page number): the page size is fixed at 50, so the
              // Nth page is start = (N - 1) * 50.
              start: 0
            },
            requestId: B24Js.Text.getUuidRfc4122() // optional unique tracking id for this request
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const tasks = response.getData().result.tasks
          // getTotal() is deprecated (removed in SDK 2.0); for the full match count fetch
          // everything via a list helper (see above) and read its length.
          console.info(`Loaded ${tasks.length} tasks (one page)`)
          for (const task of tasks) {
            console.info(`#${task.id}: ${task.title}`)
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listTasks)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.list',
                [
                    'order' => [
                        'DEADLINE' => 'asc',
                        'PRIORITY' => 'desc'
                    ],
                    'filter' => [
                        '!STATUS' => 6,
                        '>=DEADLINE' => date('Y-m-d'),
                        'RESPONSIBLE_ID' => 547,
                        '::SUBFILTER-PARAMS' => ['FAVORITE' => 'Y']
                    ],
                    'select' => [
                        'ID', 'TITLE', 'DESCRIPTION', 'STATUS', 'subStatus',
                        'DEADLINE', 'CREATED_DATE', 'RESPONSIBLE_ID',
                        'ACCOMPLICES', 'AUDITORS', 'TAGS', 'COUNTERS',
                        'PRIORITY', 'MARK'
                    ],
                    'params' => [
                        'WITH_TIMER_INFO' => true,
                        'WITH_RESULT_INFO' => true,
                        'WITH_PARSED_DESCRIPTION' => true,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching tasks: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.task.list',
        {
            // Sorting
            order: {
                'DEADLINE': 'asc',
                'PRIORITY': 'desc'
            },
            // Filtering
            filter: {
                '!STATUS': 6, // Exclude deferred tasks
                '>=DEADLINE': new Date().toISOString().split('T')[0], // Not overdue
                'RESPONSIBLE_ID': 547, // Tasks of a specific responsible person
                '::SUBFILTER-PARAMS': { 'FAVORITE': 'Y' } // Favorite tasks only
            },
            // Fields to select
            select: [
                'ID',
                'TITLE',
                'DESCRIPTION',
                'STATUS',
                'subStatus',
                'DEADLINE',
                'CREATED_DATE',
                'RESPONSIBLE_ID',
                'ACCOMPLICES',
                'AUDITORS',
                'TAGS',
                'COUNTERS',
                'PRIORITY',
                'MARK'
            ],
            // Additional parameters
            params: {
                'WITH_TIMER_INFO': true,
                'WITH_RESULT_INFO': true,
                'WITH_PARSED_DESCRIPTION': true,
            },
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.list',
        [
            'order' => [
                'DEADLINE' => 'asc',
                'PRIORITY' => 'desc'
            ],
            'filter' => [
                '!STATUS' => 6,
                '>=DEADLINE' => date('Y-m-d'),
                'RESPONSIBLE_ID' => 547,
                '::SUBFILTER-PARAMS' => ['FAVORITE' => 'Y']
            ],
            'select' => [
                'ID', 'TITLE', 'DESCRIPTION', 'STATUS', 'subStatus',
                'DEADLINE', 'CREATED_DATE', 'RESPONSIBLE_ID',
                'ACCOMPLICES', 'AUDITORS', 'TAGS', 'COUNTERS',
                'PRIORITY', 'MARK'
            ],
            'params' => [
                'WITH_TIMER_INFO' => true,
                'WITH_RESULT_INFO' => true,
                'WITH_PARSED_DESCRIPTION' => true,
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "tasks": [
            {
                "id": "8017",
                "title": "Task Example",
                "description": "Task description with [B]formatting[/B]",
                "deadline": "2025-10-24T19:00:00+02:00",
                "createdDate": "2025-06-04T16:15:55+02:00",
                "responsibleId": "547",
                "priority": "2",
                "mark": "",
                "descriptionInBbcode": "Y",
                "lengthDeadline": "1",
                "status": "2",
                "auditors": [
                    "13",
                    "103"
                ],
                "accomplices": [],
                "group": [],
                "responsible": {
                    "id": "547",
                    "name": "Maria",
                    "link": "/company/personal/user/547/",
                    "icon": "/bitrix/images/tasks/default_avatar.png",
                    "workPosition": "Tester"
                },
                "accomplicesData": [],
                "auditorsData": {
                    "13": {
                        "id": "13",
                        "name": "John Smith",
                        "link": "/company/personal/user/13/",
                        "icon": "https://mysite.com/b17053/resize_cache/209/c0120a8d7c10d63c83e32398d1ec4d9e/main/c8dd225a1c6ea0a25722d01644b90fe4/8b.jpg",
                        "workPosition": "System Administrator"
                    },
                    "103": {
                        "id": "103",
                        "name": "Samantha Johnson",
                        "link": "/company/personal/user/103/",
                        "icon": "https://mysite.com/b17053/resize_cache/8644/c0120a8d7c10d63c83e32398d1ec4d9e/main/45f/45fff10d17d398a5583184c8350cd197/buh.jpg",
                        "workPosition": "Accountant"
                    }
                },
                "taskRequireResult": "Y",
                "taskHasOpenResult": "N",
                "taskHasResult": "Y",
                "timeElapsed": null,
                "timerIsRunningForCurrentUser": "N",
                "parsedDescription": "Task description with [B]formatting[/B]",
                "counter": {
                    "counters": {
                        "expired": 0,
                        "newComments": 0,
                        "projectExpired": 0,
                        "projectNewComments": 0,
                        "mutedExpired": 0,
                        "mutedNewComments": 0
                    },
                    "color": "gray",
                    "value": 0
                },
                "tags": {
                    "35": {
                        "id": 35,
                        "title": "arpar"
                    }
                },
                "subStatus": "2"
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1761054322,
        "finish": 1761054322.348041,
        "duration": 0.3480410575866699,
        "processing": 0,
        "date_start": "2025-10-21T16:45:22+02:00",
        "date_finish": "2025-10-21T16:45:22+02:00",
        "operating_reset_at": 1761054922,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | An object containing the response data ||
|| **tasks**
[`object`](../data-types.md) | An array of objects, where each object contains a [task description](./fields.md).

The set of fields depends on the `select` parameter ||
|| **total**
[`integer`](../data-types.md) | Total number of records found ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Invalid sorting key (internal error)"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Invalid sorting key (internal error) | The `order` parameter contains a task field that cannot be sorted or a non-existent field ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-task-add.md)
- [{#T}](./tasks-task-update.md)
- [{#T}](./tasks-task-get.md)
- [{#T}](./tasks-task-delete.md)
- [{#T}](./tasks-task-get-fields.md)
- [{#T}](../../tutorials/tasks/how-to-create-comment-with-file.md)
- [{#T}](../../tutorials/tasks/how-to-upload-file-to-task.md)
- [{#T}](../../tutorials/tasks/how-to-create-task-with-file.md)