# How to Complete Business Processes of a Terminated Employee

> Scope: [`user_brief, user_basic, user, bizproc`](../../api-reference/scopes/permissions.md)
> 
> Who can execute methods: administrator

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

When an employee is terminated in Bitrix24, there may be unfinished business processes for which they were responsible.

To complete the active business processes of a terminated employee, we will sequentially execute three methods:

1. [user.get](../../api-reference/user/user-get.md) — retrieve the `ID` of the terminated employee

2. [bizproc.task.list](../../api-reference/bizproc/bizproc-task/bizproc-task-list.md) — obtain a list of process tasks for which the terminated employee is responsible

3. [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) — complete the business processes while deleting data. If you need to retain the fact that the business process was initiated, use the method [bizproc.workflow.terminate](../../api-reference/bizproc/bizproc-workflow-terminate.md). Both methods are called in the same way.

## 1. Retrieve the ID of the Terminated Employee {#user-id}

We will use the method [user.get](../../api-reference/user/user-get.md) with the following filter:

- `NAME` — specify the employee's first name

- `LAST_NAME` — specify the employee's last name

- `ACTIVE` — this parameter controls the search for active or terminated employees. If this parameter is not provided, the search will include all employees regardless of their status. Specify `0` to search only among terminated employees

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/1/xxxxxxxxxxxxxxxx/')

    const response = await $b24.actions.v2.call.make({
        method: 'user.get',
        params: {
            filter: {
                NAME: "employee's name",
                LAST_NAME: "employee's last name",
                ACTIVE: 0,
            },
        },
        requestId: 'user-get',
    })

    const users = response.getData().result
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $b24 = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/1/xxxxxxxxxxxxxxxx/');

    $users = $b24->getUserScope()->user()->get(
        [],
        [
            'NAME' => "employee's name",
            'LAST_NAME' => "employee's last name",
            'ACTIVE' => 0,
        ]
    )->getUsers();
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )
    client = Client(token)

    result = client.user.get(
        filter={
            "NAME": "employee's name",
            "LAST_NAME": "employee's last name",
            "ACTIVE": 0,
        }
    ).response.result
    ```

{% endlist %}

As a result, we will obtain the `ID` of the terminated employee.

```json
{
    "result": [
        {
            "ID": "29",
            "ACTIVE": false,
            "NAME": "employee's name",
            "LAST_NAME": "employee's last name",
            "EMAIL": "employee_email@gmail.com",
            "WORK_POSITION": "Manager",
            "UF_DEPARTMENT": [
                7,
                1
            ],
            "USER_TYPE": "employee"
        }
    ],
    "total": 1,
}
```

## 2. Retrieve the List of Process Tasks for Which the Terminated Employee is Responsible {#workflow_id}

We will use the method [bizproc.task.list](../../api-reference/bizproc/bizproc-task/bizproc-task-list.md) with the following filter:

- `USER_ID` — the employee identifier; pass the ID obtained in [step 1](#user-id)

- `STATUS` — this parameter handles the assignment status; specify `0` to select only uncompleted assignments

{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'bizproc.task.list',
        params: {
            filter: {
                USER_ID: 29,
                STATUS: 0,
            },
        },
        requestId: 'bizproc-task-list',
    })

    const tasks = response.getData().result
    ```

- PHP

    ```php
    $tasks = $b24->getBizProcScope()->task()->list(
        [],
        [
            'USER_ID' => 29,
            'STATUS' => 0,
        ]
    )->getTasks();
    ```

- Python

    ```python
    result = client.bizproc.task.list(
        filter={
            "USER_ID": 29,
            "STATUS": 0,
        }
    ).response.result
    ```

{% endlist %}

As a result, we will obtain a list of incomplete tasks. Each task has a `WORKFLOW_ID` parameter — this is the `ID` of the business process that we will complete in the next step.

```json
{
    "result": [
        {
            "ENTITY": "CCrmDocumentContact",
            "DOCUMENT_ID": "CONTACT_2437",
            "ID": "879",
            "WORKFLOW_ID": "67e3db8e581121.72266518",
            "DOCUMENT_NAME": "widget contact",
            "NAME": "Address",
            "DOCUMENT_URL": "/crm/contact/details/2437/"
        }
    ],
    "total": 1,
}
```

## 3. Terminate the Workflows

Use the [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) method with the following parameter:

- `ID` — the process identifier; pass the `WORKFLOW_ID` obtained in [step 2](#workflow_id)
  
{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'bizproc.workflow.kill',
        params: { ID: '67e3db8e581121.72266518' },
        requestId: 'bizproc-workflow-kill',
    })

    const isKilled = response.getData().result
    ```

- PHP

    ```php
    $isKilled = $b24->getBizProcScope()->workflow()
        ->kill('67e3db8e581121.72266518')
        ->isSuccess();
    ```

- Python

    ```python
    # Business process ID — a string, rather than the typed client.bizproc.workflow.kill
    # expects an int, so we call the method directly via token.call_method
    result = token.call_method(
        "bizproc.workflow.kill",
        {"ID": "67e3db8e581121.72266518"},
    )
    ```

{% endlist %}

{% note warning "" %}

In b24pysdk, the typed method `client.bizproc.workflow.kill(bitrix_id=...)` expects an integer `bitrix_id`, but the business process identifier is a string like `67e3db8e581121.72266518`. Therefore, to terminate the process, use the universal call `token.call_method("bizproc.workflow.kill", {"ID": workflow_id})`, where `token` is the `BitrixWebhook` object.

{% endnote %}

As a result, you will obtain `true`, the process deletion was successful. If you received an error `error`, study the description of possible errors in the [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) method documentation.

```json
{
    "result": true,
}
```

## Code Example

In the example, all found processes are deleted within a loop. If you need to delete a large volume of data, you may encounter request execution limits. To optimize the code for your workload, use the recommendations in the [Performance](../../settings/performance/index.md) section.

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/1/xxxxxxxxxxxxxxxx/')

    async function getUserId(firstName, lastName) {
        const response = await $b24.actions.v2.call.make({
            method: 'user.get',
            params: { filter: { NAME: firstName, LAST_NAME: lastName, ACTIVE: 0 } },
            requestId: 'user-get',
        })
        if (!response.isSuccess) throw new Error(response.getErrorMessages().join('; '))
        const users = response.getData().result
        return users.length ? users[0].ID : null
    }

    async function getWorkflowIds(userId) {
        const response = await $b24.actions.v2.call.make({
            method: 'bizproc.task.list',
            params: { filter: { USER_ID: userId, STATUS: 0 } },
            requestId: 'bizproc-task-list',
        })
        if (!response.isSuccess) throw new Error(response.getErrorMessages().join('; '))
        return response.getData().result.map((task) => task.WORKFLOW_ID)
    }

    async function killWorkflows(workflowIds) {
        for (const workflowId of workflowIds) {
            const response = await $b24.actions.v2.call.make({
                method: 'bizproc.workflow.kill',
                params: { ID: workflowId },
                requestId: `kill-${workflowId}`,
            })
            console.log(response.isSuccess
                ? `Workflow ${workflowId} completed successfully.`
                : `Error: ${response.getErrorMessages().join('; ')}`)
        }
    }

    // Employee first and last name are passed as arguments: node kill.mjs Klaus Weber
    const [firstName, lastName] = process.argv.slice(2)
    const userId = await getUserId(firstName, lastName)
    if (userId) {
        await killWorkflows(await getWorkflowIds(userId))
    }
    $b24.destroy()
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Bitrix24\SDK\Services\ServiceBuilder;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $b24 = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/1/xxxxxxxxxxxxxxxx/');

    function getUserId(ServiceBuilder $b24, string $firstName, string $lastName): ?int
    {
        $users = $b24->getUserScope()->user()->get(
            [],
            ['NAME' => $firstName, 'LAST_NAME' => $lastName, 'ACTIVE' => 0]
        )->getUsers();

        return $users === [] ? null : $users[0]->ID;
    }

    function getWorkflowIds(ServiceBuilder $b24, int $userId): array
    {
        $tasks = $b24->getBizProcScope()->task()->list(
            [],
            ['USER_ID' => $userId, 'STATUS' => 0]
        )->getTasks();

        return array_map(static fn($task) => $task->WORKFLOW_ID, $tasks);
    }

    function killWorkflows(ServiceBuilder $b24, array $workflowIds): void
    {
        foreach ($workflowIds as $workflowId) {
            $isKilled = $b24->getBizProcScope()->workflow()->kill($workflowId)->isSuccess();
            echo $isKilled
                ? "Workflow {$workflowId} completed successfully.\n"
                : "Error deleting process {$workflowId}\n";
        }
    }

    $firstName = readline('Enter employee\'s first name: ');
    $lastName = readline('Enter employee\'s last name: ');

    $userId = getUserId($b24, $firstName, $lastName);
    if ($userId !== null) {
        killWorkflows($b24, getWorkflowIds($b24, $userId));
    }
    ```

- Python

    ```python
    from typing import Optional

    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    def get_user_id(client, first_name: str, last_name: str) -> Optional[int]:
        try:
            users = client.user.get(
                filter={
                    "NAME": first_name,
                    "LAST_NAME": last_name,
                    "ACTIVE": 0,
                },
            ).response.result
        except BitrixAPIError as error:
            print(f"Error: {error}")
            return None

        if not users:
            return None
        return int(users[0]["ID"])

    def get_user_tasks(client, user_id: int) -> list[str]:
        tasks = client.bizproc.task.list(
            filter={
                "USER_ID": user_id,
                "STATUS": 0,
            },
        ).response.result

        return [task["WORKFLOW_ID"] for task in tasks]

    def kill_workflows(token, workflow_ids: list[str]) -> None:
        # Process ID is a string, so we use the universal token.call_method,
        # instead of the typed client.bizproc.workflow.kill (it expects an int)
        for workflow_id in workflow_ids:
            try:
                token.call_method("bizproc.workflow.kill", {"ID": workflow_id})
            except BitrixAPIError as error:
                print(f"Error: {error}")
            else:
                print(f"Workflow {workflow_id} completed successfully.")

    def process_employee_tasks(client, token, first_name: str, last_name: str) -> None:
        user_id = get_user_id(client, first_name, last_name)
        if user_id is None:
            return
        workflow_ids = get_user_tasks(client, user_id)
        kill_workflows(token, workflow_ids)

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )
    client = Client(token)

    first_name = input("Enter employee's first name: ")
    last_name = input("Enter employee's last name: ")

    process_employee_tasks(client, token, first_name, last_name)
    ```

{% endlist %}
