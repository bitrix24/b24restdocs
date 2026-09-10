# How to Delegate Incomplete Tasks of a Terminated Employee

> Scope: [`task`, `user_basic`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, you need permission to view and delegate tasks
>
> - [user.get](../../api-reference/user/user-get.md) - any user
> - [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) - a user with access to the tasks from the filter
> - [tasks.task.getaccess](../../api-reference/tasks/tasks-task-get-access.md) - any user
> - [tasks.task.delegate](../../api-reference/tasks/tasks-task-delegate.md) - a user with permission to delegate the task

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

After an employee is terminated, their incomplete tasks can be delegated to another responsible person. To do this, find the terminated employee identifier, retrieve the tasks where this employee is responsible, and delegate each task to the new employee.

Delegation depends on the permissions of the user who created the webhook. Before calling the method that changes the responsible person, first retrieve the tasks and then check the `DELEGATE` permission for each of them.

The scenario consists of four steps.

1. Find the terminated employee and the new responsible person using [user.get](../../api-reference/user/user-get.md)
2. Retrieve the terminated employee's incomplete tasks using [tasks.task.list](../../api-reference/tasks/tasks-task-list.md)
3. Check delegation permission using [tasks.task.getaccess](../../api-reference/tasks/tasks-task-get-access.md)
4. Delegate each accessible task using [tasks.task.delegate](../../api-reference/tasks/tasks-task-delegate.md)

As a result, the responsible person will change for tasks where the user who created the webhook has the `DELEGATE` permission. Completed tasks are not delegated: the delegation method returns the `Task already completed` error.

## Before You Start

Check access conditions and prepare the input data:

- an inbound webhook is created for a user who can see the terminated employee's tasks and has permission to delegate these tasks

- the webhook permissions include the `task` and `user_basic` scopes

- you know the name, last name, e-mail, or another attribute of the terminated employee

- you know the name, last name, e-mail, or identifier of the new responsible person

The webhook runs requests with the permissions of the user who created it. An administrator sees all tasks, a manager sees their employees' tasks, and other users see only tasks available to them.

Store the webhook URL in an environment variable and do not publish it in open code.

In the examples below, we search for the terminated employee by the last name `Weber` and for the new responsible person by the e-mail `new.responsible@example.com`. Replace these values with your own values in your Bitrix24.

Server-side JS examples with `B24Hook` require Node.js 18, 20, 22, or later. For new projects, use 22 or later. B24JsSDK is an ES module: save the code in an `.mjs` file or add `"type": "module"` to `package.json`.

Examples with b24pysdk require Python 3.9 or later.

## 1. Find Employees

The [user.get](../../api-reference/user/user-get.md) method retrieves users by filter. To find the terminated employee, pass `ACTIVE: false` and an additional attribute to the filter, such as a last name or e-mail. To find the new responsible person, pass `ACTIVE: true`.

Save these values from the response:

- `ID` of the terminated employee - we will pass it to the `RESPONSIBLE_ID` task filter
- `ID` of the new responsible person - we will pass it to the `userId` parameter of the delegation method

{% include [Examples Note](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

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

    async function findOneUser(filter, requestId) {
        const result = await callMethod(
            'user.get',
            {
                FILTER: filter,
                SELECT: ['ID', 'ACTIVE', 'NAME', 'LAST_NAME', 'EMAIL']
            },
            requestId
        )

        if (!Array.isArray(result) || result.length !== 1) {
            throw new Error(`Expected one user, received: ${Array.isArray(result) ? result.length : 0}`)
        }

        return result[0]
    }

    const firedUser = await findOneUser(
        {
            ACTIVE: false,
            LAST_NAME: 'Weber'
        },
        'user-get-fired'
    )

    const newResponsible = await findOneUser(
        {
            ACTIVE: true,
            EMAIL: 'new.responsible@example.com'
        },
        'user-get-new-responsible'
    )
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

    function callMethod($serviceBuilder, string $method, array $params): array
    {
        return $serviceBuilder
            ->core
            ->call($method, $params)
            ->getResponseData()
            ->getResult();
    }

    function findOneUser($serviceBuilder, array $filter): array
    {
        $result = callMethod(
            $serviceBuilder,
            'user.get',
            [
                'FILTER' => $filter,
                'SELECT' => ['ID', 'ACTIVE', 'NAME', 'LAST_NAME', 'EMAIL'],
            ]
        );

        if (count($result) !== 1) {
            throw new RuntimeException('Expected one user, received: ' . count($result));
        }

        return $result[0];
    }

    $firedUser = findOneUser(
        $serviceBuilder,
        [
            'ACTIVE' => false,
            'LAST_NAME' => 'Weber',
        ]
    );

    $newResponsible = findOneUser(
        $serviceBuilder,
        [
            'ACTIVE' => true,
            'EMAIL' => 'new.responsible@example.com',
        ]
    );
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

    def find_one_user(filter):
        result = client.user.get(
            filter=filter,
            select=["ID", "ACTIVE", "NAME", "LAST_NAME", "EMAIL"],
        ).response.result

        if len(result) != 1:
            raise RuntimeError(f"Expected one user, received: {len(result)}")

        return result[0]

    fired_user = find_one_user(
        {
            "ACTIVE": False,
            "LAST_NAME": "Weber",
        }
    )

    new_responsible = find_one_user(
        {
            "ACTIVE": True,
            "EMAIL": "new.responsible@example.com",
        }
    )
    ```

{% endlist %}

Short response for the terminated employee:

```json
{
    "result": [
        {
            "ID": "37",
            "ACTIVE": false,
            "NAME": "Klaus",
            "LAST_NAME": "Weber",
            "EMAIL": "k.weber@example.com"
        }
    ],
    "total": 1
}
```

Short response for the new responsible person:

```json
{
    "result": [
        {
            "ID": "547",
            "ACTIVE": true,
            "NAME": "Anna",
            "LAST_NAME": "Schmidt",
            "EMAIL": "new.responsible@example.com"
        }
    ],
    "total": 1
}
```

As a result, we retrieved the terminated employee `ID` `37` and the new responsible person `ID` `547`. These values are required in the next steps.

## 2. Retrieve the Terminated Employee's Tasks

The [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) method retrieves tasks with page navigation. Pass these values to the filter:

- `RESPONSIBLE_ID` - terminated employee identifier
- `!REAL_STATUS: 5` - exclude completed tasks

Completed tasks must be excluded before delegation. The [tasks.task.delegate](../../api-reference/tasks/tasks-task-delegate.md) method does not perform the action on a completed task.

{% list tabs %}

- JS

    ```javascript
    async function getEmployeeTasks(employeeId) {
        const tasks = []
        let start = 0

        while (true) {
            const result = await callMethod(
                'tasks.task.list',
                {
                    order: { ID: 'asc' },
                    filter: {
                        RESPONSIBLE_ID: Number(employeeId),
                        '!REAL_STATUS': 5
                    },
                    select: ['ID', 'TITLE', 'RESPONSIBLE_ID', 'STATUS'],
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

    const tasks = await getEmployeeTasks(firedUser.ID)
    ```

- PHP

    ```php
    function getEmployeeTasks($serviceBuilder, int $employeeId): array
    {
        $tasks = [];
        $start = 0;

        do {
            $result = callMethod(
                $serviceBuilder,
                'tasks.task.list',
                [
                    'order' => ['ID' => 'asc'],
                    'filter' => [
                        'RESPONSIBLE_ID' => $employeeId,
                        '!REAL_STATUS' => 5,
                    ],
                    'select' => ['ID', 'TITLE', 'RESPONSIBLE_ID', 'STATUS'],
                    'start' => $start,
                ]
            );

            $page = $result['tasks'] ?? [];
            $tasks = array_merge($tasks, $page);
            $start += 50;
        } while (count($page) === 50);

        return $tasks;
    }

    $tasks = getEmployeeTasks($serviceBuilder, (int)$firedUser['ID']);
    ```

- Python

    ```python
    def get_employee_tasks(employee_id):
        tasks = []
        start = 0

        while True:
            result = client.tasks.task.list(
                order={"ID": "asc"},
                filter={
                    "RESPONSIBLE_ID": int(employee_id),
                    "!REAL_STATUS": 5,
                },
                select=["ID", "TITLE", "RESPONSIBLE_ID", "STATUS"],
                start=start,
            ).response.result

            page = result.get("tasks", [])
            tasks.extend(page)

            if len(page) < 50:
                break

            start += 50

        return tasks

    tasks = get_employee_tasks(fired_user["ID"])
    ```

{% endlist %}

Short response:

```json
{
    "result": {
        "tasks": [
            {
                "id": "8017",
                "title": "Prepare the presentation",
                "responsibleId": "37",
                "status": "2"
            }
        ]
    },
    "total": 1
}
```

As a result, we retrieved an array of tasks. For the next step, you need each task's `id`. If the method returns 50 tasks, request the next page with `start: 50`.

## 3. Check Delegation Permission

The [tasks.task.getaccess](../../api-reference/tasks/tasks-task-get-access.md) method checks which actions are available to the user who created the webhook. Before changing tasks in bulk, check the `DELEGATE` action for each task from step 2.

If `DELEGATE` is `false` for a task, do not pass it to [tasks.task.delegate](../../api-reference/tasks/tasks-task-delegate.md). This task will remain assigned to the previous responsible person until a user with the required permissions delegates it manually or through another webhook.

{% list tabs %}

- JS

    ```javascript
    async function filterTasksAllowedToDelegate(tasks) {
        const allowed = []

        for (const task of tasks) {
            const result = await callMethod(
                'tasks.task.getaccess',
                {
                    taskId: Number(task.id)
                },
                `tasks-task-getaccess-${task.id}`
            )

            const currentUserActions = Object.values(result.allowedActions ?? {})[0] ?? {}

            if (currentUserActions.DELEGATE === true) {
                allowed.push(task)
            }
        }

        return allowed
    }

    const allowedTasks = await filterTasksAllowedToDelegate(tasks)
    ```

- PHP

    ```php
    function filterTasksAllowedToDelegate($serviceBuilder, array $tasks): array
    {
        $allowed = [];

        foreach ($tasks as $task) {
            $result = callMethod(
                $serviceBuilder,
                'tasks.task.getaccess',
                [
                    'taskId' => (int)$task['id'],
                ]
            );

            $allowedActions = $result['allowedActions'] ?? [];
            $currentUserActions = reset($allowedActions);

            if (is_array($currentUserActions) && ($currentUserActions['DELEGATE'] ?? false) === true) {
                $allowed[] = $task;
            }
        }

        return $allowed;
    }

    $allowedTasks = filterTasksAllowedToDelegate($serviceBuilder, $tasks);
    ```

- Python

    ```python
    def filter_tasks_allowed_to_delegate(tasks):
        allowed = []

        for task in tasks:
            result = client.tasks.task.getaccess(
                task_id=int(task["id"]),
            ).response.result

            allowed_actions = result.get("allowedActions", {})
            current_user_actions = next(iter(allowed_actions.values()), {})

            if current_user_actions.get("DELEGATE") is True:
                allowed.append(task)

        return allowed

    allowed_tasks = filter_tasks_allowed_to_delegate(tasks)
    ```

{% endlist %}

Short response:

```json
{
    "result": {
        "allowedActions": {
            "1269": {
                "DELEGATE": true
            }
        }
    }
}
```

The method returns the `allowedActions` object with the list of available actions for the task. In the example, the code checks the `DELEGATE` action: if the value is `true`, the task remains in the `allowedTasks` array.

Pass the `allowedTasks` array to step 4. If it is empty after the check, finish the scenario without calling [tasks.task.delegate](../../api-reference/tasks/tasks-task-delegate.md).

## 4. Delegate Tasks to the New Responsible Person

The [tasks.task.delegate](../../api-reference/tasks/tasks-task-delegate.md) method changes the responsible person in one task. To delegate all accessible tasks, call the method for each `id` from the `allowedTasks` array.

Pass these parameters:

- `taskId` - task identifier
- `userId` - new responsible person identifier

{% list tabs %}

- JS

    ```javascript
    async function delegateTasks(tasks, userId) {
        const delegated = []

        for (const task of tasks) {
            const result = await callMethod(
                'tasks.task.delegate',
                {
                    taskId: Number(task.id),
                    userId: Number(userId)
                },
                `tasks-task-delegate-${task.id}`
            )

            delegated.push(result.task)
        }

        return delegated
    }

    const delegatedTasks = await delegateTasks(allowedTasks, newResponsible.ID)
    console.table(delegatedTasks.map((task) => ({
        id: task.id,
        title: task.title,
        responsibleId: task.responsibleId
    })))
    ```

- PHP

    ```php
    function delegateTasks($serviceBuilder, array $tasks, int $userId): array
    {
        $delegated = [];

        foreach ($tasks as $task) {
            $result = callMethod(
                $serviceBuilder,
                'tasks.task.delegate',
                [
                    'taskId' => (int)$task['id'],
                    'userId' => $userId,
                ]
            );

            $delegated[] = $result['task'];
        }

        return $delegated;
    }

    $delegatedTasks = delegateTasks($serviceBuilder, $allowedTasks, (int)$newResponsible['ID']);
    print_r($delegatedTasks);
    ```

- Python

    ```python
    def delegate_tasks(tasks, user_id):
        delegated = []

        for task in tasks:
            result = client.tasks.task.delegate(
                task_id=int(task["id"]),
                user_id=int(user_id),
            ).response.result

            delegated.append(result["task"])

        return delegated

    delegated_tasks = delegate_tasks(allowed_tasks, new_responsible["ID"])
    print(delegated_tasks)
    ```

{% endlist %}

Short response:

```json
{
    "result": {
        "task": {
            "id": "8017",
            "title": "Prepare the presentation",
            "responsibleId": "547",
            "status": "2"
        }
    }
}
```

The method returns the `task` object with updated task data. If `responsibleId` in the response equals the identifier of the new responsible person, the task has been delegated.

In the example, the code stores delegated tasks in the `delegatedTasks` array. This array is required to verify the result.

## Verify the Result

Verify the result by calling [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) again: select tasks by the `RESPONSIBLE_ID` of the new responsible person and by the delegated task identifiers.

{% list tabs %}

- JS

    ```javascript
    const delegatedTaskIds = delegatedTasks.map((task) => task.id)

    if (delegatedTaskIds.length > 0) {
        const checkResult = await callMethod(
            'tasks.task.list',
            {
                filter: {
                    '@ID': delegatedTaskIds,
                    RESPONSIBLE_ID: Number(newResponsible.ID)
                },
                select: ['ID', 'TITLE', 'RESPONSIBLE_ID']
            },
            'tasks-task-list-check'
        )

        console.table(checkResult.tasks)
    }
    ```

- PHP

    ```php
    $delegatedTaskIds = array_map(
        static fn(array $task): string => (string)$task['id'],
        $delegatedTasks
    );

    if (!empty($delegatedTaskIds)) {
        $checkResult = callMethod(
            $serviceBuilder,
            'tasks.task.list',
            [
                'filter' => [
                    '@ID' => $delegatedTaskIds,
                    'RESPONSIBLE_ID' => (int)$newResponsible['ID'],
                ],
                'select' => ['ID', 'TITLE', 'RESPONSIBLE_ID'],
            ]
        );

        print_r($checkResult['tasks'] ?? []);
    }
    ```

- Python

    ```python
    delegated_task_ids = [task["id"] for task in delegated_tasks]

    if delegated_task_ids:
        check_result = client.tasks.task.list(
            filter={
                "@ID": delegated_task_ids,
                "RESPONSIBLE_ID": int(new_responsible["ID"]),
            },
            select=["ID", "TITLE", "RESPONSIBLE_ID"],
        ).response.result

        print(check_result.get("tasks", []))
    ```

{% endlist %}

The scenario is successful if the number of tasks in the verification response matches the number of delegated tasks and each task's `responsibleId` equals the new responsible person `ID`.

Check these values in the responses:

- all tasks from the `allowedTasks` array are present in the `delegatedTasks` array

- each `responsibleId` value in the [tasks.task.delegate](../../api-reference/tasks/tasks-task-delegate.md) response equals the new responsible person `ID`

- the verification call to [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) returned the same tasks by the `@ID` and `RESPONSIBLE_ID` filters

In the interface, the tasks will appear for the new responsible person.

## Errors and Troubleshooting

If the method returns an error, check the request data.

#|
|| **Code or Error Text** | **Reason and Action** ||
|| `insufficient_scope` | The webhook does not have the required scope. Task methods require the `task` scope. [user.get](../../api-reference/user/user-get.md) with e-mail search requires `user_basic` or `user` ||
|| `Action on task is not allowed` | The user who created the webhook does not have permission to delegate the task. Check action permission using [tasks.task.getaccess](../../api-reference/tasks/tasks-task-get-access.md) ||
|| `Task already completed` | The task is completed. Exclude completed tasks using the `!REAL_STATUS: 5` filter ||
|| `Could not find value for parameter {userId}` | The new responsible person identifier was not passed in the `userId` parameter ||
|| `wrong task id` | An invalid task identifier was passed in the `taskId` parameter ||
|#

If the task list is empty, check:

- the terminated employee was found using [user.get](../../api-reference/user/user-get.md), and their `ID` was passed to `RESPONSIBLE_ID`
- the user who created the webhook can see the terminated employee's tasks
- the [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) filter does not contain extra conditions
- the terminated employee has incomplete tasks that can be delegated

If `allowedTasks` is empty while the task list is not empty, the user who created the webhook does not have the `DELEGATE` permission for these tasks. Run the scenario through a webhook created by a user with the required permissions, or delegate the tasks manually in the interface.

If some tasks have already been delegated, restart the scenario from step 2. The [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) method will no longer return delegated tasks by the terminated employee `RESPONSIBLE_ID` filter.

## Key Points

Before running the scenario, consider method and access permission limitations.

- [tasks.task.list](../../api-reference/tasks/tasks-task-list.md) returns tasks in pages of 50 items. To retrieve all tasks, iterate over `start`: `0`, `50`, `100`
- [tasks.task.delegate](../../api-reference/tasks/tasks-task-delegate.md) delegates one task per call. To delegate a list of tasks, call the method for each task
- completed tasks cannot be delegated. The scenario delegates only incomplete tasks
- delegation depends on the permissions of the user who created the webhook. If the user does not have access to the task or permission to delegate it, the method returns an error
- if several employees are found by the [user.get](../../api-reference/user/user-get.md) filter, refine the filter: add `EMAIL`, `ID`, or another exact attribute

## Continue Learning

- [{#T}](../../api-reference/user/user-get.md)
- [{#T}](../../api-reference/tasks/tasks-task-list.md)
- [{#T}](../../api-reference/tasks/tasks-task-get-access.md)
- [{#T}](../../api-reference/tasks/tasks-task-delegate.md)
- [{#T}](../../api-reference/tasks/index.md)
