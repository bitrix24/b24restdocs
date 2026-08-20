# How to Terminate Workflows of a Dismissed Employee

> Scope: [`user_brief`](../../api-reference/scopes/permissions.md), [`user_basic`](../../api-reference/scopes/permissions.md), [`user`](../../api-reference/scopes/permissions.md), [`bizproc`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: administrator permissions are required to complete the whole scenario
>
> - [user.get](../../api-reference/user/user-get.md) — any user
> - [bizproc.task.list](../../api-reference/bizproc/bizproc-task/bizproc-task-list.md) — administrator, to view tasks of any user
> - [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) and [bizproc.workflow.terminate](../../api-reference/bizproc/bizproc-workflow-terminate.md) — administrator

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

After an employee is dismissed in Bitrix24, there may still be incomplete workflow tasks assigned to them. You can get the related `WORKFLOW_ID` values from those tasks and terminate the linked workflows.

The `bizproc.workflow.kill` method deletes the workflow together with its data. If you need to stop execution but keep the record of the workflow launch, use [bizproc.workflow.terminate](../../api-reference/bizproc/bizproc-workflow-terminate.md). Both methods accept the same workflow identifier.

The scenario consists of three steps.

1. Get the dismissed employee `ID` using the [user.get](../../api-reference/user/user-get.md) method
2. Get the employee's incomplete tasks using the [bizproc.task.list](../../api-reference/bizproc/bizproc-task/bizproc-task-list.md) method
3. Delete the related workflows using the [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) method

## Before You Start

- an administrator inbound webhook or an application with the `bizproc` scope and one of the user scopes: `user`, `user_basic`, or `user_brief`
- the first and last name of the dismissed employee
- a decision on what to do with the found workflows: delete them with `bizproc.workflow.kill` or stop them with `bizproc.workflow.terminate`

An administrator can retrieve tasks of any user. A regular user can see only their own tasks or the tasks of a subordinate, so administrator authorization is required for the full scenario.

{% include [Note on examples](../../_includes/examples.md) %}

## 1. Get the ID of the Dismissed Employee {#user-id}

Use the [user.get](../../api-reference/user/user-get.md) method with the following filter:

- `NAME` — the employee's first name
- `LAST_NAME` — the employee's last name
- `ACTIVE = 0` — search only among dismissed employees

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const response = await $b24.actions.v2.call.make({
        method: 'user.get',
        params: {
            filter: {
                NAME: 'Klaus',
                LAST_NAME: 'Weber',
                ACTIVE: 0,
            },
        },
        requestId: 'user-get',
    })

    const users = response.getData().result
    const userId = users.length ? Number(users[0].ID) : null
    ```

- PHP

    ```php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $b24 = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $users = $b24->getUserScope()->user()->get(
        [],
        [
            'NAME' => 'Klaus',
            'LAST_NAME' => 'Weber',
            'ACTIVE' => 0,
        ]
    )->getUsers();

    $userId = $users === [] ? null : $users[0]->ID;
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )
    client = Client(token)

    users = client.user.get(
        filter={
            "NAME": "Klaus",
            "LAST_NAME": "Weber",
            "ACTIVE": 0,
        }
    ).response.result

    user_id = int(users[0]["ID"]) if users else None
    ```

- Go

    ```go
    import (
        "context"
        "encoding/json"
        "log"
        "os"

        b24 "github.com/bitrix24/b24gosdk"
    )

    ctx := context.Background()
    core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

    res, err := core.Call(ctx, "user.get", b24.Params{
        "filter": b24.Params{
            "NAME":      "Klaus",
            "LAST_NAME": "Weber",
            "ACTIVE":    0,
        },
    }, b24.WithIdempotent())
    if err != nil {
        log.Fatal(err)
    }

    var users []struct {
        ID string `json:"ID"`
    }
    if err := json.Unmarshal(res.Result, &users); err != nil {
        log.Fatal(err)
    }

    userID := ""
    if len(users) > 0 {
        userID = users[0].ID
    }
    ```

{% endlist %}

Store the employee `ID` from the response. You need this value for the `USER_ID` filter in the next step.

```json
{
    "result": [
        {
            "ID": "29",
            "ACTIVE": false,
            "NAME": "Klaus",
            "LAST_NAME": "Weber",
            "EMAIL": "employee@example.com",
            "USER_TYPE": "employee"
        }
    ],
    "total": 1
}
```

If `result` contains an empty array, the dismissed employee with the specified first and last name was not found. Verify the search data and repeat the first step.

## 2. Get the Employee's Tasks {#workflow-id}

Use the [bizproc.task.list](../../api-reference/bizproc/bizproc-task/bizproc-task-list.md) method with the following filter:

- `USER_ID` — the employee identifier from [step 1](#user-id)
- `STATUS = 0` — only incomplete tasks

{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'bizproc.task.list',
        params: {
            select: ['ID', 'WORKFLOW_ID', 'NAME', 'DOCUMENT_NAME'],
            filter: {
                USER_ID: userId,
                STATUS: 0,
            },
        },
        requestId: 'bizproc-task-list',
    })

    const tasks = response.getData().result
    const workflowIds = [...new Set(tasks.map((task) => task.WORKFLOW_ID))]
    ```

- PHP

    ```php
    $response = $b24->core->call('bizproc.task.list', [
        'select' => ['ID', 'WORKFLOW_ID', 'NAME', 'DOCUMENT_NAME'],
        'filter' => [
            'USER_ID' => $userId,
            'STATUS' => 0,
        ],
    ]);

    $tasks = $response->getResponseData()->getResult();
    $workflowIds = array_values(array_unique(array_column($tasks, 'WORKFLOW_ID')));
    ```

- Python

    ```python
    tasks = client.bizproc.task.list(
        select=["ID", "WORKFLOW_ID", "NAME", "DOCUMENT_NAME"],
        filter={
            "USER_ID": user_id,
            "STATUS": 0,
        },
    ).response.result

    workflow_ids = list({task["WORKFLOW_ID"] for task in tasks})
    ```

- Go

    ```go
    res, err := core.Call(ctx, "bizproc.task.list", b24.Params{
        "select": []string{"ID", "WORKFLOW_ID", "NAME", "DOCUMENT_NAME"},
        "filter": b24.Params{
            "USER_ID": userID,
            "STATUS":  0,
        },
    }, b24.WithIdempotent())
    if err != nil {
        log.Fatal(err)
    }

    var tasks []struct {
        WorkflowID string `json:"WORKFLOW_ID"`
    }
    if err := json.Unmarshal(res.Result, &tasks); err != nil {
        log.Fatal(err)
    }

    seen := map[string]struct{}{}
    workflowIDs := []string{}
    for _, task := range tasks {
        if _, ok := seen[task.WorkflowID]; ok {
            continue
        }

        seen[task.WorkflowID] = struct{}{}
        workflowIDs = append(workflowIDs, task.WorkflowID)
    }
    ```

{% endlist %}

Store `WORKFLOW_ID` from the response. This is the string identifier of the workflow that must be passed to the `ID` parameter of the `bizproc.workflow.kill` method.

```json
{
    "result": [
        {
            "ID": "879",
            "WORKFLOW_ID": "67e3db8e581121.72266518",
            "DOCUMENT_NAME": "Client Contact",
            "NAME": "Approve Address"
        }
    ],
    "total": 1
}
```

If `result` contains an empty array, the employee has no incomplete workflow tasks. There is nothing to terminate in this scenario.

## 3. Terminate the Workflows

Use the [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) method with the following parameter:

- `ID` — the workflow identifier. Pass `WORKFLOW_ID` from [step 2](#workflow-id)

{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'bizproc.workflow.kill',
        params: { ID: workflowIds[0] },
        requestId: 'bizproc-workflow-kill',
    })

    const isKilled = response.getData().result
    ```

- PHP

    ```php
    $isKilled = $b24->getBizProcScope()->workflow()
        ->kill($workflowIds[0])
        ->isSuccess();
    ```

- Python

    ```python
    token.call_method(
        "bizproc.workflow.kill",
        {"ID": workflow_ids[0]},
    )
    ```

- Go

    ```go
    res, err := core.Call(ctx, "bizproc.workflow.kill", b24.Params{
        "ID": workflowIDs[0],
    })
    if err != nil {
        log.Fatal(err)
    }

    var isKilled bool
    if err := json.Unmarshal(res.Result, &isKilled); err != nil {
        log.Fatal(err)
    }
    log.Println(isKilled)
    ```

{% endlist %}

The successful response contains `true`.

```json
{
    "result": true
}
```

## Code Example

The example first prints the found workflows for review. To delete the workflows, run the example with the `--confirm` argument.

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)

    async function getUserId(firstName, lastName) {
        const response = await $b24.actions.v2.call.make({
            method: 'user.get',
            params: { filter: { NAME: firstName, LAST_NAME: lastName, ACTIVE: 0 } },
            requestId: 'user-get',
        })

        const users = response.getData().result
        return users.length ? Number(users[0].ID) : null
    }

    async function getWorkflowIds(userId) {
        const response = await $b24.actions.v2.call.make({
            method: 'bizproc.task.list',
            params: {
                select: ['ID', 'WORKFLOW_ID', 'NAME', 'DOCUMENT_NAME'],
                filter: { USER_ID: userId, STATUS: 0 },
            },
            requestId: 'bizproc-task-list',
        })

        return [...new Set(response.getData().result.map((task) => task.WORKFLOW_ID))]
    }

    async function killWorkflows(workflowIds) {
        if (!workflowIds.length) {
            console.log('No incomplete workflow tasks found')
            return
        }

        console.log(`Found workflows: ${workflowIds.length}`)
        for (const workflowId of workflowIds) {
            console.log(`Workflow to delete: ${workflowId}`)
        }

        if (!process.argv.includes('--confirm')) {
            console.log('Review the list and rerun the example with the --confirm argument to delete the workflows')
            return
        }

        for (const workflowId of workflowIds) {
            const response = await $b24.actions.v2.call.make({
                method: 'bizproc.workflow.kill',
                params: { ID: workflowId },
                requestId: `workflow-kill-${workflowId}`,
            })

            console.log(response.isSuccess
                ? `Workflow ${workflowId} deleted`
                : `Error deleting workflow ${workflowId}: ${response.getErrorMessages().join('; ')}`)
        }
    }

    const [firstName, lastName] = process.argv.slice(2)
    if (!firstName || !lastName) {
        throw new Error('Pass the employee first and last name')
    }

    const userId = await getUserId(firstName, lastName)
    if (userId === null) {
        console.log('Dismissed employee not found')
    } else {
        await killWorkflows(await getWorkflowIds(userId))
    }

    $b24.destroy()
    ```

- PHP

    ```php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilder;
    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $b24 = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

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
        $response = $b24->core->call('bizproc.task.list', [
            'select' => ['ID', 'WORKFLOW_ID', 'NAME', 'DOCUMENT_NAME'],
            'filter' => ['USER_ID' => $userId, 'STATUS' => 0],
        ]);

        return array_values(array_unique(array_column(
            $response->getResponseData()->getResult(),
            'WORKFLOW_ID'
        )));
    }

    function killWorkflows(ServiceBuilder $b24, array $workflowIds): void
    {
        if ($workflowIds === []) {
            echo "No incomplete workflow tasks found\n";
            return;
        }

        echo "Found workflows: " . count($workflowIds) . "\n";
        foreach ($workflowIds as $workflowId) {
            echo "Workflow to delete: {$workflowId}\n";
        }

        if (!in_array('--confirm', $_SERVER['argv'], true)) {
            echo "Review the list and rerun the example with the --confirm argument to delete the workflows\n";
            return;
        }

        foreach ($workflowIds as $workflowId) {
            $isKilled = $b24->getBizProcScope()->workflow()->kill($workflowId)->isSuccess();
            echo $isKilled
                ? "Workflow {$workflowId} deleted\n"
                : "Error deleting workflow {$workflowId}\n";
        }
    }

    $firstName = readline('Enter the employee first name: ');
    $lastName = readline('Enter the employee last name: ');

    $userId = getUserId($b24, $firstName, $lastName);
    if ($userId === null) {
        echo "Dismissed employee not found\n";
    } else {
        killWorkflows($b24, getWorkflowIds($b24, $userId));
    }
    ```

- Python

    ```python
    import sys

    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )
    client = Client(token)

    def get_user_id(first_name: str, last_name: str) -> int | None:
        users = client.user.get(
            filter={
                "NAME": first_name,
                "LAST_NAME": last_name,
                "ACTIVE": 0,
            },
        ).response.result

        return int(users[0]["ID"]) if users else None

    def get_workflow_ids(user_id: int) -> list[str]:
        tasks = client.bizproc.task.list(
            select=["ID", "WORKFLOW_ID", "NAME", "DOCUMENT_NAME"],
            filter={"USER_ID": user_id, "STATUS": 0},
        ).response.result

        return list({task["WORKFLOW_ID"] for task in tasks})

    def kill_workflows(workflow_ids: list[str]) -> None:
        if not workflow_ids:
            print("No incomplete workflow tasks found")
            return

        print(f"Found workflows: {len(workflow_ids)}")
        for workflow_id in workflow_ids:
            print(f"Workflow to delete: {workflow_id}")

        if "--confirm" not in sys.argv:
            print("Review the list and rerun the example with the --confirm argument to delete the workflows")
            return

        for workflow_id in workflow_ids:
            try:
                token.call_method("bizproc.workflow.kill", {"ID": workflow_id})
            except BitrixAPIError as error:
                print(f"Error deleting workflow {workflow_id}: {error}")
            else:
                print(f"Workflow {workflow_id} deleted")

    first_name = input("Enter the employee first name: ")
    last_name = input("Enter the employee last name: ")

    user_id = get_user_id(first_name, last_name)
    if user_id is None:
        print("Dismissed employee not found")
    else:
        kill_workflows(get_workflow_ids(user_id))
    ```

- Go

    ```go
    // Setup in an empty directory:
    //  go mod init example && go get github.com/bitrix24/b24gosdk
    //
    // Run:
    //  B24_WEBHOOK_URL="https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/" go run main.go Klaus Weber
    //  B24_WEBHOOK_URL="https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/" go run main.go Klaus Weber --confirm

    package main

    import (
        "context"
        "encoding/json"
        "fmt"
        "os"

        b24 "github.com/bitrix24/b24gosdk"
    )

    type user struct {
        ID string `json:"ID"`
    }

    type task struct {
        WorkflowID string `json:"WORKFLOW_ID"`
    }

    func main() {
        if err := run(); err != nil {
            fmt.Fprintf(os.Stderr, "%v\n", err)
            os.Exit(1)
        }
    }

    func run() error {
        if len(os.Args) < 3 {
            return fmt.Errorf("pass the employee first and last name")
        }

        firstName := os.Args[1]
        lastName := os.Args[2]
        confirmed := false
        for _, arg := range os.Args[3:] {
            if arg == "--confirm" {
                confirmed = true
            }
        }

        ctx := context.Background()
        core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

        userID, err := getUserID(ctx, core, firstName, lastName)
        if err != nil {
            return err
        }
        if userID == "" {
            fmt.Println("Dismissed employee not found")
            return nil
        }

        workflowIDs, err := getWorkflowIDs(ctx, core, userID)
        if err != nil {
            return err
        }
        if len(workflowIDs) == 0 {
            fmt.Println("No incomplete workflow tasks found")
            return nil
        }

        fmt.Printf("Found workflows: %d\n", len(workflowIDs))
        for _, workflowID := range workflowIDs {
            fmt.Printf("Workflow to delete: %s\n", workflowID)
        }

        if !confirmed {
            fmt.Println("Review the list and rerun the example with the --confirm argument to delete the workflows")
            return nil
        }

        for _, workflowID := range workflowIDs {
            if err := killWorkflow(ctx, core, workflowID); err != nil {
                fmt.Printf("Error deleting workflow %s: %v\n", workflowID, err)
                continue
            }

            fmt.Printf("Workflow %s deleted\n", workflowID)
        }

        return nil
    }

    func getUserID(ctx context.Context, core *b24.Core, firstName string, lastName string) (string, error) {
        res, err := core.Call(ctx, "user.get", b24.Params{
            "filter": b24.Params{
                "NAME":      firstName,
                "LAST_NAME": lastName,
                "ACTIVE":    0,
            },
        }, b24.WithIdempotent())
        if err != nil {
            return "", fmt.Errorf("user.get: %w", err)
        }

        var users []user
        if err := json.Unmarshal(res.Result, &users); err != nil {
            return "", fmt.Errorf("decode user.get: %w", err)
        }

        if len(users) == 0 {
            return "", nil
        }

        return users[0].ID, nil
    }

    func getWorkflowIDs(ctx context.Context, core *b24.Core, userID string) ([]string, error) {
        res, err := core.Call(ctx, "bizproc.task.list", b24.Params{
            "select": []string{"ID", "WORKFLOW_ID", "NAME", "DOCUMENT_NAME"},
            "filter": b24.Params{
                "USER_ID": userID,
                "STATUS":  0,
            },
        }, b24.WithIdempotent())
        if err != nil {
            return nil, fmt.Errorf("bizproc.task.list: %w", err)
        }

        var tasks []task
        if err := json.Unmarshal(res.Result, &tasks); err != nil {
            return nil, fmt.Errorf("decode bizproc.task.list: %w", err)
        }

        seen := map[string]struct{}{}
        workflowIDs := []string{}
        for _, task := range tasks {
            if _, ok := seen[task.WorkflowID]; ok {
                continue
            }

            seen[task.WorkflowID] = struct{}{}
            workflowIDs = append(workflowIDs, task.WorkflowID)
        }

        return workflowIDs, nil
    }

    func killWorkflow(ctx context.Context, core *b24.Core, workflowID string) error {
        _, err := core.Call(ctx, "bizproc.workflow.kill", b24.Params{
            "ID": workflowID,
        })
        if err != nil {
            return fmt.Errorf("bizproc.workflow.kill: %w", err)
        }

        return nil
    }
    ```

{% endlist %}

## Check the Result

In the interface, check the dismissed employee's tasks: the tasks of deleted workflows must no longer be present. If you used `bizproc.workflow.terminate` instead of deletion, the workflow must stop, but the workflow launch record must remain.

Through REST, repeat the [bizproc.task.list](../../api-reference/bizproc/bizproc-task/bizproc-task-list.md) request with the same `USER_ID` and `STATUS = 0`.

{% list tabs %}

- JS

    ```js
    const checkResponse = await $b24.actions.v2.call.make({
        method: 'bizproc.task.list',
        params: {
            select: ['ID', 'WORKFLOW_ID'],
            filter: { USER_ID: userId, STATUS: 0 },
        },
        requestId: 'bizproc-task-list-check',
    })

    console.log(checkResponse.getData().result.map((task) => task.WORKFLOW_ID))
    ```

- PHP

    ```php
    $checkResponse = $b24->core->call('bizproc.task.list', [
        'select' => ['ID', 'WORKFLOW_ID'],
        'filter' => ['USER_ID' => $userId, 'STATUS' => 0],
    ]);

    foreach ($checkResponse->getResponseData()->getResult() as $task) {
        echo $task['WORKFLOW_ID'] . PHP_EOL;
    }
    ```

- Python

    ```python
    check_result = client.bizproc.task.list(
        select=["ID", "WORKFLOW_ID"],
        filter={"USER_ID": user_id, "STATUS": 0},
    ).response.result

    print([task["WORKFLOW_ID"] for task in check_result])
    ```

- Go

    ```go
    res, err := core.Call(ctx, "bizproc.task.list", b24.Params{
        "select": []string{"ID", "WORKFLOW_ID"},
        "filter": b24.Params{
            "USER_ID": userID,
            "STATUS":  0,
        },
    }, b24.WithIdempotent())
    if err != nil {
        log.Fatal(err)
    }

    var tasks []struct {
        WorkflowID string `json:"WORKFLOW_ID"`
    }
    if err := json.Unmarshal(res.Result, &tasks); err != nil {
        log.Fatal(err)
    }

    for _, task := range tasks {
        log.Println(task.WorkflowID)
    }
    ```

{% endlist %}

The scenario is successful if the response does not contain the `WORKFLOW_ID` values that were passed to `bizproc.workflow.kill`.

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Error Code or Message** | **Cause and action** ||
|| `ACCESS_DENIED` | The method was called by a user without the required permissions, or the webhook does not have the required scope ||
|| `ERROR_WRONG_WORKFLOW_ID` | An empty value or a non-string value was passed in `ID` ||
|| An empty `result` array in `user.get` | The dismissed employee with the specified first and last name was not found ||
|| An empty `result` array in `bizproc.task.list` | The employee has no incomplete workflow tasks ||
|| `bizproc.*` methods are unavailable | Verify that business processes are available in Bitrix24 and that the request is executed by an administrator ||
|#

Repeat the scenario from the step where the error occurred. If the error occurred while deleting one workflow, verify its `WORKFLOW_ID` and continue processing the rest.

## What to Consider

- `bizproc.workflow.kill` deletes the workflow together with its data
- `bizproc.workflow.terminate` stops the workflow and keeps the record of its launch
- the user identifier from [user.get](../../api-reference/user/user-get.md) is passed as `USER_ID` to the [bizproc.task.list](../../api-reference/bizproc/bizproc-task/bizproc-task-list.md) method
- `WORKFLOW_ID` from [bizproc.task.list](../../api-reference/bizproc/bizproc-task/bizproc-task-list.md) is passed as `ID` to the [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) method
- `WORKFLOW_ID` is a string like `67e3db8e581121.72266518`; do not convert it to a number

## Continue Learning

- [Get the List of Users by Filter](../../api-reference/user/user-get.md)
- [Get the List of Workflow Tasks](../../api-reference/bizproc/bizproc-task/bizproc-task-list.md)
- [Delete a Running Workflow](../../api-reference/bizproc/bizproc-workflow-kill.md)
- [Stop an Active Workflow](../../api-reference/bizproc/bizproc-workflow-terminate.md)
