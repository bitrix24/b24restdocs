# How to Calculate Time Spent on Tasks for Each Employee

> Scope: [`task`, `user_brief`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, you need permission to view tasks and time tracking records
>
> - [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) - a user with access to the tasks from the filter
> - [task.elapseditem.getlist](../../api-reference/tasks/elapsed-item/task-elapsed-item-get-list.md) - a user with access to the task
> - [user.get](../../api-reference/user/user-get.md) - any user

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Standard task reports show the total time spent on a task and can attribute the total to the responsible person. If several employees worked on the task, such a report does not show how much time each participant spent.

To get a personal time-spent table, work with time tracking records rather than with the task total. The scenario consists of three steps.

1. Retrieve tasks using [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) with a filter by project, dates, or participants
2. Retrieve time tracking records for each task using [task.elapseditem.getlist](../../api-reference/tasks/elapsed-item/task-elapsed-item-get-list.md) and group them by `USER_ID`
3. Retrieve employee names using [user.get](../../api-reference/user/user-get.md) by the identifiers from time tracking records

The result is a table where each employee row contains only that employee's time, not the total for all task participants.

## Before You Start

- an incoming webhook is created for a user who can see the required tasks and time tracking records. Task methods respect this user's permissions: the user will receive only accessible tasks

- the webhook permissions include the `task` and `user_brief` scopes

- you know the workgroup ID or another filter used to select tasks

- you know the report period, for example task completion dates

- time tracking is enabled in the selected tasks, and time tracking records exist

- the webhook URL grants full access within its scope. Store the URL in an environment variable and do not publish it in open code

The webhook runs requests with the permissions of the user who created it. An administrator sees all tasks, a manager sees their employees' tasks, and other users see only tasks available to them.

The examples below use workgroup `15`, the period from `2026-08-01` to `2026-08-31`, and employee `37`. These values will be different in your Bitrix24: replace them with your own values or values from the interface.

Server-side JS examples with `B24Hook` require Node.js 18, 20, 22, or later. For new projects, use 22 or later. B24JsSDK is an ES module: save the code in an `.mjs` file or add `"type": "module"` to `package.json`.

Examples with b24pysdk require Python 3.9 or later.

## 1. Retrieve the Task List

The [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) method retrieves tasks with page navigation. In the example, we select completed workgroup tasks for a period and request the fields required for the report:

- `ID` - task identifier
- `TITLE` - task title for report row details
- `GROUP_ID` - workgroup identifier
- `CLOSED_DATE` - task completion date

If you need to build a report only for tasks where an employee is an accomplice, add the `ACCOMPLICE` field to `filter`. If you need to select tasks where the employee is responsible, use `RESPONSIBLE_ID`.

{% include [Examples Note](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const reportFilter = {
        GROUP_ID: 15,
        REAL_STATUS: 5,
        '>=CLOSED_DATE': '2026-08-01',
        '<=CLOSED_DATE': '2026-08-31'
    }

    async function callMethod(method, params, requestId) {
        const response = await $b24.actions.v2.call.make({
            method,
            params,
            requestId
        })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }

    async function getTasks(filter) {
        const tasks = []
        let start = 0

        while (true) {
            const result = await callMethod(
                'tasks.task.list',
                {
                    order: { ID: 'asc' },
                    filter,
                    select: [
                        'ID',
                        'TITLE',
                        'GROUP_ID',
                        'CLOSED_DATE'
                    ],
                    start
                },
                `tasks-task-list-${start}`
            )

            const page = result.tasks ?? []
            tasks.push(...page)

            if (page.length < 50) {
                break
            }

            start += 50
        }

        return tasks
    }
    ```

- PHP

    ```php
    <?php

    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Psr\Log\NullLogger;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    $reportFilter = [
        'GROUP_ID' => 15,
        'REAL_STATUS' => 5,
        '>=CLOSED_DATE' => '2026-08-01',
        '<=CLOSED_DATE' => '2026-08-31',
    ];

    function callMethod($serviceBuilder, string $method, array $params): array
    {
        return $serviceBuilder
            ->core
            ->call($method, $params)
            ->getResponseData()
            ->getResult();
    }

    function getTasks($serviceBuilder, array $filter): array
    {
        $tasks = [];
        $start = 0;

        do {
            $result = callMethod(
                $serviceBuilder,
                'tasks.task.list',
                [
                    'order' => ['ID' => 'asc'],
                    'filter' => $filter,
                    'select' => [
                        'ID',
                        'TITLE',
                        'GROUP_ID',
                        'CLOSED_DATE',
                    ],
                    'start' => $start,
                ]
            );

            $page = $result['tasks'] ?? [];
            $tasks = array_merge($tasks, $page);
            $start += 50;
        } while (count($page) === 50);

        return $tasks;
    }
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token=os.environ["B24_HOOK_TOKEN"],
        )
    )
    # B24_HOOK_TOKEN = 'user_id/webhook_key'

    report_filter = {
        "GROUP_ID": 15,
        "REAL_STATUS": 5,
        ">=CLOSED_DATE": "2026-08-01",
        "<=CLOSED_DATE": "2026-08-31",
    }

    def get_tasks(filter):
        tasks = []
        start = 0

        while True:
            result = client.tasks.task.list(
                order={"ID": "asc"},
                filter=filter,
                select=[
                    "ID",
                    "TITLE",
                    "GROUP_ID",
                    "CLOSED_DATE",
                ],
                start=start,
            ).response.result

            page = result.get("tasks", [])
            tasks.extend(page)

            if len(page) < 50:
                break

            start += 50

        return tasks
    ```

{% endlist %}

After this step, save the task array. For the next step, you need each task's `id`. Task participant roles are not required for the calculation: the report is built from time tracking records, not from `RESPONSIBLE_ID`, `ACCOMPLICES`, or `AUDITORS`.

Short response:

```json
{
    "result": {
        "tasks": [
            {
                "id": "8017",
                "title": "Prepare the presentation",
                "groupId": "15",
                "closedDate": "2026-08-15T18:30:00+02:00"
            }
        ]
    },
    "total": 1
}
```

As a result, we retrieved the task list for the selected period. Take the `id` from each task: these values are passed as the first parameter in step 2.

## 2. Retrieve Time for Each Task and Group It by Employee

The [task.elapseditem.getlist](../../api-reference/tasks/elapsed-item/task-elapsed-item-get-list.md) method returns time tracking records for a task. The method accepts parameters in strict order:

1. `taskId` - task identifier from step 1
2. `order` - record sorting
3. `filter` - record filter
4. `select` - time record fields. For the report, use `TASK_ID`, `USER_ID`, `SECONDS`, `MINUTES`, `COMMENT_TEXT`, and `CREATED_DATE`
5. `params` - `NAV_PARAMS` page navigation parameters. The page size is limited to 50 records

If you pass `order`, `filter`, `select`, and `params` as named fields of a single object, the request will fail. In the JS and PHP examples below, parameters are passed as a positional array. The Python SDK accepts named arguments and builds the REST call itself.

If the report is required for all employees, do not pass `USER_ID` in the filter. Retrieve all records for each task and group them by `USER_ID` in code. If you need to check one employee, pass the employee identifier in `USER_ID`.

{% list tabs %}

- JS

    ```javascript
    async function getElapsedItems(taskId, userId = null) {
        const items = []
        let pageNumber = 1

        while (true) {
            const result = await callMethod(
                'task.elapseditem.getlist',
                [
                    Number(taskId),
                    { ID: 'asc' },
                    userId ? { USER_ID: Number(userId) } : {},
                    [
                        'ID',
                        'TASK_ID',
                        'USER_ID',
                        'SECONDS',
                        'MINUTES',
                        'COMMENT_TEXT',
                        'CREATED_DATE'
                    ],
                    {
                        NAV_PARAMS: {
                            nPageSize: 50,
                            iNumPage: pageNumber
                        }
                    }
                ],
                `task-elapseditem-getlist-${taskId}-${pageNumber}`
            )

            const page = Array.isArray(result) ? result : []
            items.push(...page)

            if (page.length < 50) {
                break
            }

            pageNumber += 1
        }

        return items
    }

    function addToReport(report, task, item) {
        const userId = String(item.USER_ID)

        if (!report.has(userId)) {
            report.set(userId, {
                userId,
                seconds: 0,
                minutes: 0,
                tasks: new Map()
            })
        }

        const row = report.get(userId)
        const seconds = Number(item.SECONDS ?? 0)
        row.seconds += seconds
        row.minutes = Math.round(row.seconds / 60)

        if (!row.tasks.has(task.id)) {
            row.tasks.set(task.id, {
                taskId: task.id,
                title: task.title,
                seconds: 0,
                minutes: 0
            })
        }

        const taskRow = row.tasks.get(task.id)
        taskRow.seconds += seconds
        taskRow.minutes = Math.round(taskRow.seconds / 60)
    }

    async function buildEmployeeTimeReport(userId = null) {
        const tasks = await getTasks(reportFilter)
        const report = new Map()

        for (const task of tasks) {
            const elapsedItems = await getElapsedItems(task.id, userId)

            for (const item of elapsedItems) {
                if (userId && String(item.USER_ID) !== String(userId)) {
                    continue
                }

                addToReport(report, task, item)
            }
        }

        return [...report.values()].map((row) => ({
            userId: row.userId,
            minutes: row.minutes,
            hours: Math.round((row.seconds / 3600) * 100) / 100,
            tasks: [...row.tasks.values()]
        }))
    }
    ```

- PHP

    ```php
    function getElapsedItems($serviceBuilder, int $taskId, ?int $userId = null): array
    {
        $items = [];
        $pageNumber = 1;

        do {
            $result = callMethod(
                $serviceBuilder,
                'task.elapseditem.getlist',
                [
                    $taskId,
                    ['ID' => 'asc'],
                    $userId ? ['USER_ID' => $userId] : [],
                    [
                        'ID',
                        'TASK_ID',
                        'USER_ID',
                        'SECONDS',
                        'MINUTES',
                        'COMMENT_TEXT',
                        'CREATED_DATE',
                    ],
                    [
                        'NAV_PARAMS' => [
                            'nPageSize' => 50,
                            'iNumPage' => $pageNumber,
                        ],
                    ],
                ]
            );

            $page = is_array($result) ? $result : [];
            $items = array_merge($items, $page);
            $pageNumber++;
        } while (count($page) === 50);

        return $items;
    }

    function addToReport(array &$report, array $task, array $item): void
    {
        $userId = (string)$item['USER_ID'];
        $taskId = (string)$task['id'];
        $seconds = (int)($item['SECONDS'] ?? 0);

        if (!isset($report[$userId])) {
            $report[$userId] = [
                'userId' => $userId,
                'seconds' => 0,
                'minutes' => 0,
                'tasks' => [],
            ];
        }

        $report[$userId]['seconds'] += $seconds;
        $report[$userId]['minutes'] = (int)round($report[$userId]['seconds'] / 60);

        if (!isset($report[$userId]['tasks'][$taskId])) {
            $report[$userId]['tasks'][$taskId] = [
                'taskId' => $taskId,
                'title' => $task['title'],
                'seconds' => 0,
                'minutes' => 0,
            ];
        }

        $report[$userId]['tasks'][$taskId]['seconds'] += $seconds;
        $report[$userId]['tasks'][$taskId]['minutes'] =
            (int)round($report[$userId]['tasks'][$taskId]['seconds'] / 60);
    }

    function buildEmployeeTimeReport($serviceBuilder, array $filter, ?int $userId = null): array
    {
        $tasks = getTasks($serviceBuilder, $filter);
        $report = [];

        foreach ($tasks as $task) {
            $elapsedItems = getElapsedItems($serviceBuilder, (int)$task['id'], $userId);

            foreach ($elapsedItems as $item) {
                if ($userId && (string)$item['USER_ID'] !== (string)$userId) {
                    continue;
                }

                addToReport($report, $task, $item);
            }
        }

        return array_map(
            static function (array $row): array {
                $row['hours'] = round($row['seconds'] / 3600, 2);
                $row['tasks'] = array_values($row['tasks']);

                return $row;
            },
            array_values($report)
        );
    }

    ```

- Python

    ```python
    def get_elapsed_items(task_id, user_id=None):
        items = []
        page_number = 1

        while True:
            result = client.task.elapseditem.getlist(
                taskid=int(task_id),
                order={"ID": "asc"},
                filter={"USER_ID": int(user_id)} if user_id else {},
                select=[
                    "ID",
                    "TASK_ID",
                    "USER_ID",
                    "SECONDS",
                    "MINUTES",
                    "COMMENT_TEXT",
                    "CREATED_DATE",
                ],
                params={
                    "NAV_PARAMS": {
                        "nPageSize": 50,
                        "iNumPage": page_number,
                    }
                },
            ).response.result

            page = result if isinstance(result, list) else []
            items.extend(page)

            if len(page) < 50:
                break

            page_number += 1

        return items

    def add_to_report(report, task, item):
        user_id = str(item["USER_ID"])
        task_id = str(task["id"])
        seconds = int(item.get("SECONDS", 0))

        if user_id not in report:
            report[user_id] = {
                "userId": user_id,
                "seconds": 0,
                "minutes": 0,
                "tasks": {},
            }

        row = report[user_id]
        row["seconds"] += seconds
        row["minutes"] = round(row["seconds"] / 60)

        if task_id not in row["tasks"]:
            row["tasks"][task_id] = {
                "taskId": task_id,
                "title": task["title"],
                "seconds": 0,
                "minutes": 0,
            }

        task_row = row["tasks"][task_id]
        task_row["seconds"] += seconds
        task_row["minutes"] = round(task_row["seconds"] / 60)

    def build_employee_time_report(user_id=None):
        tasks = get_tasks(report_filter)
        report = {}

        for task in tasks:
            elapsed_items = get_elapsed_items(task["id"], user_id)

            for item in elapsed_items:
                if user_id and str(item["USER_ID"]) != str(user_id):
                    continue

                add_to_report(report, task, item)

        rows = []
        for row in report.values():
            rows.append({
                "userId": row["userId"],
                "minutes": row["minutes"],
                "hours": round(row["seconds"] / 3600, 2),
                "tasks": list(row["tasks"].values()),
            })

        return rows
    ```

{% endlist %}

After this step, save `USER_ID` from each time tracking record. These identifiers are required to retrieve employee names.

Short [task.elapseditem.getlist](../../api-reference/tasks/elapsed-item/task-elapsed-item-get-list.md) response:

```json
{
    "result": [
        {
            "ID": "104",
            "TASK_ID": "8017",
            "USER_ID": "37",
            "SECONDS": "14400",
            "MINUTES": "240",
            "COMMENT_TEXT": "Layout implementation",
            "CREATED_DATE": "2026-08-15T14:10:00+02:00"
        },
        {
            "ID": "105",
            "TASK_ID": "8017",
            "USER_ID": "41",
            "SECONDS": "3600",
            "MINUTES": "60",
            "COMMENT_TEXT": "Review",
            "CREATED_DATE": "2026-08-15T17:40:00+02:00"
        }
    ],
    "total": 2
}
```

As a result, we retrieved time tracking records for the task. The `USER_ID` field shows the employee who added the record, and `SECONDS` and `MINUTES` show the duration of this record. In step 3, we will retrieve employee names by unique `USER_ID` values.

## 3. Retrieve Employee Names

The [user.get](../../api-reference/user/user-get.md) method retrieves user data by filter. In the examples, the function for retrieving employee names takes a list of unique `USER_ID` values from the report and requests these fields:

- `ID` - employee identifier that matches `USER_ID` in the time tracking record
- `NAME` - employee first name
- `LAST_NAME` - employee last name

{% list tabs %}

- JS

    ```javascript
    async function getUserNames(userIds) {
        const users = new Map()
        const uniqueIds = [...new Set(userIds.map(String))]

        for (let i = 0; i < uniqueIds.length; i += 50) {
            const batch = uniqueIds.slice(i, i + 50)
            const result = await callMethod(
                'user.get',
                {
                    FILTER: { '@ID': batch },
                    SELECT: ['ID', 'NAME', 'LAST_NAME']
                },
                `user-get-${i}`
            )

            for (const user of Array.isArray(result) ? result : []) {
                users.set(String(user.ID), `${user.NAME ?? ''} ${user.LAST_NAME ?? ''}`.trim())
            }
        }

        return users
    }

    async function addUserNames(report) {
        const users = await getUserNames(report.map((row) => row.userId))

        return report.map((row) => ({
            ...row,
            userName: users.get(row.userId) ?? row.userId
        }))
    }

    const report = await addUserNames(await buildEmployeeTimeReport())
    console.table(report.map(({ userId, userName, hours }) => ({ userId, userName, hours })))

    const weberReport = await addUserNames(await buildEmployeeTimeReport(37))
    console.table(weberReport.map(({ userId, userName, hours }) => ({ userId, userName, hours })))
    ```

- PHP

    ```php
    function getUserNames($serviceBuilder, array $userIds): array
    {
        $users = [];
        $uniqueIds = array_values(array_unique(array_map('strval', $userIds)));

        foreach (array_chunk($uniqueIds, 50) as $batch) {
            $result = callMethod(
                $serviceBuilder,
                'user.get',
                [
                    'FILTER' => ['@ID' => $batch],
                    'SELECT' => ['ID', 'NAME', 'LAST_NAME'],
                ]
            );

            foreach ($result as $user) {
                $users[(string)$user['ID']] = trim(($user['NAME'] ?? '') . ' ' . ($user['LAST_NAME'] ?? ''));
            }
        }

        return $users;
    }

    function addUserNames($serviceBuilder, array $report): array
    {
        $users = getUserNames($serviceBuilder, array_column($report, 'userId'));

        return array_map(
            static function (array $row) use ($users): array {
                $row['userName'] = $users[$row['userId']] ?? $row['userId'];

                return $row;
            },
            $report
        );
    }

    $report = addUserNames($serviceBuilder, buildEmployeeTimeReport($serviceBuilder, $reportFilter));
    print_r($report);

    $weberReport = addUserNames($serviceBuilder, buildEmployeeTimeReport($serviceBuilder, $reportFilter, 37));
    print_r($weberReport);
    ```

- Python

    ```python
    def get_user_names(user_ids):
        users = {}
        unique_ids = list(dict.fromkeys(str(user_id) for user_id in user_ids))

        for offset in range(0, len(unique_ids), 50):
            batch = unique_ids[offset:offset + 50]
            result = client.user.get(
                filter={"@ID": batch},
                select=["ID", "NAME", "LAST_NAME"],
            ).response.result

            for user in result:
                users[str(user["ID"])] = f"{user.get('NAME', '')} {user.get('LAST_NAME', '')}".strip()

        return users

    def add_user_names(report):
        users = get_user_names(row["userId"] for row in report)

        return [
            {
                **row,
                "userName": users.get(row["userId"], row["userId"]),
            }
            for row in report
        ]

    report = add_user_names(build_employee_time_report())
    print(report)

    weber_report = add_user_names(build_employee_time_report(37))
    print(weber_report)
    ```

{% endlist %}

Short [user.get](../../api-reference/user/user-get.md) response:

```json
{
    "result": [
        {
            "ID": "37",
            "NAME": "Klaus",
            "LAST_NAME": "Weber"
        },
        {
            "ID": "41",
            "NAME": "Anna",
            "LAST_NAME": "Schmidt"
        }
    ],
    "total": 2
}
```

As a result, we retrieved employee data. We use `NAME` and `LAST_NAME` to label report rows. If a user is not found, leave their `USER_ID` in the table.

Final report table:

#|
|| **Employee** | **Minutes** | **Hours** ||
|| Klaus Weber (`37`) | `480` | `8` ||
|| Anna Schmidt (`41`) | `2580` | `43` ||
|#

If the report is required only for Klaus Weber with `USER_ID = 37`, pass `37` to `buildEmployeeTimeReport` in JS and PHP or to `build_employee_time_report` in Python. Then the final total will be calculated only from records where `USER_ID` equals `37`.

## Check the Result

Sum the `SECONDS` field for all [task.elapseditem.getlist](../../api-reference/tasks/elapsed-item/task-elapsed-item-get-list.md) records with the same `USER_ID`. The resulting total must match the `Hours` column in the final table after converting seconds to hours. The employee name must match the user returned by [user.get](../../api-reference/user/user-get.md) for this `USER_ID`.

To check one employee, call [task.elapseditem.getlist](../../api-reference/tasks/elapsed-item/task-elapsed-item-get-list.md) for each task with this filter:

```json
[
    8017,
    {
        "ID": "asc"
    },
    {
        "USER_ID": 37
    },
    [
        "ID",
        "TASK_ID",
        "USER_ID",
        "SECONDS",
        "MINUTES"
    ],
    {
        "NAV_PARAMS": {
            "nPageSize": 50,
            "iNumPage": 1
        }
    }
]
```

The scenario is successful if the final employee row includes only that employee's time tracking records. In the example, tasks could contain 51 hours across all participants, but employee `37` will show only 8 hours if their records total `28800` seconds.

## Errors and Troubleshooting

If the method returns an error, check the request data.

#|
|| **Code** | **Reason and Action** ||
|| `0` | The `order` parameter of [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) contains a field that cannot be used for sorting. Use fields from the `order` parameter description ||
|| `ERROR_CORE` | The [task.elapseditem.getlist](../../api-reference/tasks/elapsed-item/task-elapsed-item-get-list.md) method could not execute the action. Check whether the webhook user has access to the task ||
|| `0x100002` | Access denied. Check the webhook user's permissions for the task ||
|| `0x000100` | Invalid [task.elapseditem.getlist](../../api-reference/tasks/elapsed-item/task-elapsed-item-get-list.md) method parameters were provided. Check the order of positional parameters: `taskId`, `order`, `filter`, `select`, `params` ||
|| `insufficient_scope` | The webhook does not have the required scope. [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) and [task.elapseditem.getlist](../../api-reference/tasks/elapsed-item/task-elapsed-item-get-list.md) require the `task` scope. [user.get](../../api-reference/user/user-get.md) requires `user_brief`, `user_basic`, or `user` ||
|#

If the final table is empty, check:

- the [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) filter actually returns tasks for the selected period
- time tracking is enabled in the tasks, and time tracking records exist
- the webhook user can see these tasks
- the correct task identifier is passed as the first parameter to [task.elapseditem.getlist](../../api-reference/tasks/elapsed-item/task-elapsed-item-get-list.md)
- the `USER_ID` filter does not exclude the required records
- the webhook has the `task` scope for task methods and the `user_brief` scope if the report must include employee names

Repeat the scenario from the step where the error occurred. If [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) returned the error, fix the filter and repeat step 1. If [task.elapseditem.getlist](../../api-reference/tasks/elapsed-item/task-elapsed-item-get-list.md) returned the error, repeat step 2 for the task with the correct identifier and parameter order. If [user.get](../../api-reference/user/user-get.md) returned the error, add the required scope and repeat step 3.

## Key Points

- [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) returns tasks in pages of 50 items. To retrieve the entire period, iterate over `start`: `0`, `50`, `100`
- [task.elapseditem.getlist](../../api-reference/tasks/elapsed-item/task-elapsed-item-get-list.md) limits the page size to 50 records through `PARAMS[NAV_PARAMS][nPageSize]`
- [user.get](../../api-reference/user/user-get.md) returns users in pages of 50 items. In the example, employee identifiers are requested in batches of 50
- the `timeElapsed` field or sorting by `TIME_SPENT_IN_LOGS` refers to the task's total time. For a personal report, use [task.elapseditem.getlist](../../api-reference/tasks/elapsed-item/task-elapsed-item-get-list.md) records and grouping by `USER_ID`
- the `ACCOMPLICE` filter in [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) selects tasks where the user is an accomplice. For the responsible person, use `RESPONSIBLE_ID`
- the report counts time tracking records regardless of the user's role in the task. If you need to limit the report to responsible persons or accomplices, add the corresponding filter to [tasks.task.list](../../api-reference/tasks/tasks-task-list.md)

## Code Example

The code runs all three steps: retrieves tasks for a period, collects time tracking records for each task, groups time by `USER_ID`, and adds employee names. Replace the webhook URL, task filter, and `employeeId` if you need a report for only one employee.

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const reportFilter = {
        GROUP_ID: 15,
        REAL_STATUS: 5,
        '>=CLOSED_DATE': '2026-08-01',
        '<=CLOSED_DATE': '2026-08-31'
    }

    const employeeId = null
    // const employeeId = 37

    async function callMethod(method, params, requestId) {
        const response = await $b24.actions.v2.call.make({ method, params, requestId })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }

    async function getTasks(filter) {
        const tasks = []
        let start = 0

        while (true) {
            const result = await callMethod(
                'tasks.task.list',
                {
                    order: { ID: 'asc' },
                    filter,
                    select: ['ID', 'TITLE', 'GROUP_ID', 'CLOSED_DATE'],
                    start
                },
                `tasks-task-list-${start}`
            )

            const page = result.tasks ?? []
            tasks.push(...page)

            if (page.length < 50) {
                break
            }

            start += 50
        }

        return tasks
    }

    async function getElapsedItems(taskId, userId = null) {
        const items = []
        let pageNumber = 1

        while (true) {
            const result = await callMethod(
                'task.elapseditem.getlist',
                [
                    Number(taskId),
                    { ID: 'asc' },
                    userId ? { USER_ID: Number(userId) } : {},
                    ['ID', 'TASK_ID', 'USER_ID', 'SECONDS', 'MINUTES', 'COMMENT_TEXT', 'CREATED_DATE'],
                    {
                        NAV_PARAMS: {
                            nPageSize: 50,
                            iNumPage: pageNumber
                        }
                    }
                ],
                `task-elapseditem-getlist-${taskId}-${pageNumber}`
            )

            const page = Array.isArray(result) ? result : []
            items.push(...page)

            if (page.length < 50) {
                break
            }

            pageNumber += 1
        }

        return items
    }

    function addToReport(report, task, item) {
        const userId = String(item.USER_ID)
        const taskId = String(task.id)
        const seconds = Number(item.SECONDS ?? 0)

        if (!report.has(userId)) {
            report.set(userId, {
                userId,
                seconds: 0,
                minutes: 0,
                tasks: new Map()
            })
        }

        const row = report.get(userId)
        row.seconds += seconds
        row.minutes = Math.round(row.seconds / 60)

        if (!row.tasks.has(taskId)) {
            row.tasks.set(taskId, {
                taskId,
                title: task.title,
                seconds: 0,
                minutes: 0
            })
        }

        const taskRow = row.tasks.get(taskId)
        taskRow.seconds += seconds
        taskRow.minutes = Math.round(taskRow.seconds / 60)
    }

    async function getUserNames(userIds) {
        const users = new Map()
        const uniqueIds = [...new Set(userIds.map(String))]

        for (let i = 0; i < uniqueIds.length; i += 50) {
            const batch = uniqueIds.slice(i, i + 50)
            const result = await callMethod(
                'user.get',
                {
                    FILTER: { '@ID': batch },
                    SELECT: ['ID', 'NAME', 'LAST_NAME']
                },
                `user-get-${i}`
            )

            for (const user of Array.isArray(result) ? result : []) {
                users.set(String(user.ID), `${user.NAME ?? ''} ${user.LAST_NAME ?? ''}`.trim())
            }
        }

        return users
    }

    async function buildReport(userId = null) {
        const tasks = await getTasks(reportFilter)
        const report = new Map()

        for (const task of tasks) {
            const elapsedItems = await getElapsedItems(task.id, userId)

            for (const item of elapsedItems) {
                addToReport(report, task, item)
            }
        }

        const rows = [...report.values()].map((row) => ({
            userId: row.userId,
            minutes: row.minutes,
            hours: Math.round((row.seconds / 3600) * 100) / 100,
            tasks: [...row.tasks.values()]
        }))

        const users = await getUserNames(rows.map((row) => row.userId))

        return rows.map((row) => ({
            ...row,
            userName: users.get(row.userId) ?? row.userId
        }))
    }

    console.table(await buildReport(employeeId))
    ```

- PHP

    ```php
    <?php

    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Psr\Log\NullLogger;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    $reportFilter = [
        'GROUP_ID' => 15,
        'REAL_STATUS' => 5,
        '>=CLOSED_DATE' => '2026-08-01',
        '<=CLOSED_DATE' => '2026-08-31',
    ];

    $employeeId = null;
    // $employeeId = 37;

    function callMethod($serviceBuilder, string $method, array $params): array
    {
        return $serviceBuilder
            ->core
            ->call($method, $params)
            ->getResponseData()
            ->getResult();
    }

    function getTasks($serviceBuilder, array $filter): array
    {
        $tasks = [];
        $start = 0;

        do {
            $result = callMethod(
                $serviceBuilder,
                'tasks.task.list',
                [
                    'order' => ['ID' => 'asc'],
                    'filter' => $filter,
                    'select' => ['ID', 'TITLE', 'GROUP_ID', 'CLOSED_DATE'],
                    'start' => $start,
                ]
            );

            $page = $result['tasks'] ?? [];
            $tasks = array_merge($tasks, $page);
            $start += 50;
        } while (count($page) === 50);

        return $tasks;
    }

    function getElapsedItems($serviceBuilder, int $taskId, ?int $userId = null): array
    {
        $items = [];
        $pageNumber = 1;

        do {
            $result = callMethod(
                $serviceBuilder,
                'task.elapseditem.getlist',
                [
                    $taskId,
                    ['ID' => 'asc'],
                    $userId ? ['USER_ID' => $userId] : [],
                    ['ID', 'TASK_ID', 'USER_ID', 'SECONDS', 'MINUTES', 'COMMENT_TEXT', 'CREATED_DATE'],
                    [
                        'NAV_PARAMS' => [
                            'nPageSize' => 50,
                            'iNumPage' => $pageNumber,
                        ],
                    ],
                ]
            );

            $page = is_array($result) ? $result : [];
            $items = array_merge($items, $page);
            $pageNumber++;
        } while (count($page) === 50);

        return $items;
    }

    function addToReport(array &$report, array $task, array $item): void
    {
        $userId = (string)$item['USER_ID'];
        $taskId = (string)$task['id'];
        $seconds = (int)($item['SECONDS'] ?? 0);

        if (!isset($report[$userId])) {
            $report[$userId] = [
                'userId' => $userId,
                'seconds' => 0,
                'minutes' => 0,
                'tasks' => [],
            ];
        }

        $report[$userId]['seconds'] += $seconds;
        $report[$userId]['minutes'] = (int)round($report[$userId]['seconds'] / 60);

        if (!isset($report[$userId]['tasks'][$taskId])) {
            $report[$userId]['tasks'][$taskId] = [
                'taskId' => $taskId,
                'title' => $task['title'],
                'seconds' => 0,
                'minutes' => 0,
            ];
        }

        $report[$userId]['tasks'][$taskId]['seconds'] += $seconds;
        $report[$userId]['tasks'][$taskId]['minutes'] =
            (int)round($report[$userId]['tasks'][$taskId]['seconds'] / 60);
    }

    function getUserNames($serviceBuilder, array $userIds): array
    {
        $users = [];
        $uniqueIds = array_values(array_unique(array_map('strval', $userIds)));

        foreach (array_chunk($uniqueIds, 50) as $batch) {
            $result = callMethod(
                $serviceBuilder,
                'user.get',
                [
                    'FILTER' => ['@ID' => $batch],
                    'SELECT' => ['ID', 'NAME', 'LAST_NAME'],
                ]
            );

            foreach ($result as $user) {
                $users[(string)$user['ID']] = trim(($user['NAME'] ?? '') . ' ' . ($user['LAST_NAME'] ?? ''));
            }
        }

        return $users;
    }

    function buildReport($serviceBuilder, array $filter, ?int $userId = null): array
    {
        $report = [];

        foreach (getTasks($serviceBuilder, $filter) as $task) {
            foreach (getElapsedItems($serviceBuilder, (int)$task['id'], $userId) as $item) {
                addToReport($report, $task, $item);
            }
        }

        $userNames = getUserNames($serviceBuilder, array_keys($report));

        return array_map(
            static function (array $row) use ($userNames): array {
                $row['hours'] = round($row['seconds'] / 3600, 2);
                $row['tasks'] = array_values($row['tasks']);
                $row['userName'] = $userNames[$row['userId']] ?? $row['userId'];

                return $row;
            },
            array_values($report)
        );
    }

    print_r(buildReport($serviceBuilder, $reportFilter, $employeeId));
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token=os.environ["B24_HOOK_TOKEN"],
        )
    )
    # B24_HOOK_TOKEN = 'user_id/webhook_key'

    report_filter = {
        "GROUP_ID": 15,
        "REAL_STATUS": 5,
        ">=CLOSED_DATE": "2026-08-01",
        "<=CLOSED_DATE": "2026-08-31",
    }

    employee_id = None
    # employee_id = 37

    def get_tasks(filter):
        tasks = []
        start = 0

        while True:
            result = client.tasks.task.list(
                order={"ID": "asc"},
                filter=filter,
                select=["ID", "TITLE", "GROUP_ID", "CLOSED_DATE"],
                start=start,
            ).response.result

            page = result.get("tasks", [])
            tasks.extend(page)

            if len(page) < 50:
                break

            start += 50

        return tasks

    def get_elapsed_items(task_id, user_id=None):
        items = []
        page_number = 1

        while True:
            result = client.task.elapseditem.getlist(
                taskid=int(task_id),
                order={"ID": "asc"},
                filter={"USER_ID": int(user_id)} if user_id else {},
                select=["ID", "TASK_ID", "USER_ID", "SECONDS", "MINUTES", "COMMENT_TEXT", "CREATED_DATE"],
                params={
                    "NAV_PARAMS": {
                        "nPageSize": 50,
                        "iNumPage": page_number,
                    }
                },
            ).response.result

            page = result if isinstance(result, list) else []
            items.extend(page)

            if len(page) < 50:
                break

            page_number += 1

        return items

    def add_to_report(report, task, item):
        user_id = str(item["USER_ID"])
        task_id = str(task["id"])
        seconds = int(item.get("SECONDS", 0))

        report.setdefault(
            user_id,
            {
                "userId": user_id,
                "seconds": 0,
                "minutes": 0,
                "tasks": {},
            },
        )

        report[user_id]["seconds"] += seconds
        report[user_id]["minutes"] = round(report[user_id]["seconds"] / 60)

        report[user_id]["tasks"].setdefault(
            task_id,
            {
                "taskId": task_id,
                "title": task["title"],
                "seconds": 0,
                "minutes": 0,
            },
        )

        report[user_id]["tasks"][task_id]["seconds"] += seconds
        report[user_id]["tasks"][task_id]["minutes"] = round(
            report[user_id]["tasks"][task_id]["seconds"] / 60
        )

    def get_user_names(user_ids):
        users = {}
        unique_ids = list(dict.fromkeys(str(user_id) for user_id in user_ids))

        for offset in range(0, len(unique_ids), 50):
            batch = unique_ids[offset:offset + 50]
            result = client.user.get(
                filter={"@ID": batch},
                select=["ID", "NAME", "LAST_NAME"],
            ).response.result

            for user in result:
                users[str(user["ID"])] = f"{user.get('NAME', '')} {user.get('LAST_NAME', '')}".strip()

        return users

    def build_report(user_id=None):
        report = {}

        for task in get_tasks(report_filter):
            for item in get_elapsed_items(task["id"], user_id):
                add_to_report(report, task, item)

        users = get_user_names(report.keys())
        rows = []

        for row in report.values():
            rows.append(
                {
                    "userId": row["userId"],
                    "userName": users.get(row["userId"], row["userId"]),
                    "minutes": row["minutes"],
                    "hours": round(row["seconds"] / 3600, 2),
                    "tasks": list(row["tasks"].values()),
                }
            )

        return rows

    print(build_report(employee_id))
    ```

{% endlist %}

## Continue Learning

- [Get a List of Tasks tasks.task.list](../../api-reference/tasks/tasks-task-list.md)
- [Get a List of Time Tracking Records task.elapseditem.getlist](../../api-reference/tasks/elapsed-item/task-elapsed-item-get-list.md)
- [Get a List of Users by Filter user.get](../../api-reference/user/user-get.md)
- [Add a Time Tracking Record task.elapseditem.add](../../api-reference/tasks/elapsed-item/task-elapsed-item-add.md)
- [Tasks: Methods Overview](../../api-reference/tasks/index.md)
