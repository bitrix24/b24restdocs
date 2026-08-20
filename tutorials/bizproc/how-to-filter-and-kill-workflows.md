# How to Mass Terminate Workflows with Date Filtering

> Scope: [`bizproc`](../../api-reference/scopes/permissions.md)
> 
> Who can execute methods: administrator

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Bitrix24 can retain running workflows that are no longer needed: they are stuck, waiting for outdated data, or were launched before a process change. You can find them by launch date and remove them using the `bizproc.workflow.kill` method.

The `bizproc.workflow.kill` method deletes the workflow together with its data. If you need to stop execution but keep the launch record, use [bizproc.workflow.terminate](../../api-reference/bizproc/bizproc-workflow-terminate.md). Both methods accept the same workflow identifier.

The scenario consists of two steps.

1. Retrieve the list of workflows using the [bizproc.workflow.instances](../../api-reference/bizproc/bizproc-workflow-instances.md) method
2. Remove the selected workflows using the [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) method

## What You Need Before You Start

- an administrator inbound webhook or an application with the `bizproc` scope
- a date before which running workflows should be found. The examples use `2025-01-01T00:00:00Z`
- a decision on what to do with the found workflows: delete them using `bizproc.workflow.kill` or stop them using `bizproc.workflow.terminate`

Before deleting, make sure to verify the found `ID` values. Re-running the same filter may find different workflows if new launches with matching dates appear in the meantime.

{% include [Note on examples](../../_includes/examples.md) %}

## 1. Retrieve the List of Processes {#workflow-id}

Use the [bizproc.workflow.instances](../../api-reference/bizproc/bizproc-workflow-instances.md) method with the following parameters:

- `filter[<STARTED]` — launch date. The `<` prefix selects workflows started before the specified time
- `select` — the fields required by the scenario. It is enough to get `ID` and `STARTED`

{% list tabs %}

- JS
  
    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const response = await $b24.actions.v2.call.make({
        method: 'bizproc.workflow.instances',
        params: {
            select: ['ID', 'STARTED'],
            filter: { '<STARTED': '2025-01-01T00:00:00Z' },
        },
        requestId: 'workflow-instances',
    })

    const instances = response.getData().result
    const workflowIds = instances.map((instance) => instance.ID)
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

    $response = $b24->core->call('bizproc.workflow.instances', [
        'select' => ['ID', 'STARTED'],
        'filter' => ['<STARTED' => '2025-01-01T00:00:00Z'],
    ]);

    $instances = $response->getResponseData()->getResult();
    $workflowIds = array_column($instances, 'ID');
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )
    client = Client(token)

    instances = client.bizproc.workflow.instances(
        select=["ID", "STARTED"],
        filter={"<STARTED": "2025-01-01T00:00:00Z"},
    ).response.result

    workflow_ids = [instance["ID"] for instance in instances]
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

    res, err := core.Call(ctx, "bizproc.workflow.instances", b24.Params{
        "select": []string{"ID", "STARTED"},
        "filter": b24.Params{"<STARTED": "2025-01-01T00:00:00Z"},
    }, b24.WithIdempotent())
    if err != nil {
        log.Fatal(err)
    }

    var instances []struct {
        ID      string `json:"ID"`
        Started string `json:"STARTED"`
    }
    if err := json.Unmarshal(res.Result, &instances); err != nil {
        log.Fatal(err)
    }

    workflowIDs := make([]string, 0, len(instances))
    for _, instance := range instances {
        workflowIDs = append(workflowIDs, instance.ID)
    }
    ```

{% endlist %}

Store the `ID` values of the workflows that need to be deleted or stopped.

```json
{
    "result": [
        {
            "ID": "660e559f34af10.95144732",
            "STARTED": "2024-12-04T10:04:24+03:00"
        },
        {
            "ID": "6639c7b59e9eb5.40607056",
            "STARTED": "2024-12-04T09:52:40+03:00"
        }
    ],
    "total": 2
}
```

If `result` contains an empty array, there are no matching workflows. There is no need to proceed to the second step.

## 2. Terminate Workflows

Use the [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) method with the following parameter:

- `ID` — the workflow identifier from the response of [step 1](#workflow-id). Pass a string such as `660e559f34af10.95144732`

{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'bizproc.workflow.kill',
        params: { ID: workflowIds[0] },
        requestId: 'workflow-kill',
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

The example first prints all found workflows for review. To delete the workflows, run the example with the `--confirm` argument. For large volumes of data, keep REST limits and the [Performance](../../settings/performance/index.md) recommendations in mind.

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)

    const [day, month, year] = (process.argv[2] || '').split('.')
    if (!day || !month || !year) {
        throw new Error('Pass the date in dd.mm.yyyy format')
    }

    const isoDate = `${year}-${month}-${day}T00:00:00Z`
    const confirmed = process.argv.includes('--confirm')

    const listResponse = await $b24.actions.v2.callList.make({
        method: 'bizproc.workflow.instances',
        params: {
            select: ['ID', 'STARTED'],
            filter: { '<STARTED': isoDate },
        },
        requestId: 'workflow-instances',
    })

    const instances = listResponse.getData()
    const workflowIds = instances.map((instance) => instance.ID)

    if (!workflowIds.length) {
        console.log('No workflows found')
        $b24.destroy()
        process.exit(0)
    }

    console.log(`Found workflows: ${workflowIds.length}`)
    for (const workflowId of workflowIds) {
        console.log(`Workflow to delete: ${workflowId}`)
    }

    if (!confirmed) {
        console.log('Review the list and rerun the example with the --confirm argument to delete the workflows')
        $b24.destroy()
        process.exit(0)
    }

    for (const workflowId of workflowIds) {
        const killResponse = await $b24.actions.v2.call.make({
            method: 'bizproc.workflow.kill',
            params: { ID: workflowId },
            requestId: `workflow-kill-${workflowId}`,
        })

        console.log(killResponse.isSuccess
            ? `Workflow ${workflowId} deleted`
            : `Error deleting workflow ${workflowId}: ${killResponse.getErrorMessages().join('; ')}`)
    }

    $b24.destroy()
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

    $userDateInput = readline('Enter the date in dd.mm.yyyy format: ');
    if (!preg_match('/^\d{2}\.\d{2}\.\d{4}$/', $userDateInput)) {
        throw new InvalidArgumentException('Enter the date in dd.mm.yyyy format');
    }

    [$day, $month, $year] = explode('.', $userDateInput);
    $isoDate = "{$year}-{$month}-{$day}T00:00:00Z";
    $confirmed = in_array('--confirm', $argv, true);
    $workflowIds = [];
    $start = 0;

    do {
        $response = $b24->core->call('bizproc.workflow.instances', [
            'select' => ['ID', 'STARTED'],
            'filter' => ['<STARTED' => $isoDate],
            'start' => $start,
        ]);

        foreach ($response->getResponseData()->getResult() as $instance) {
            $workflowIds[] = $instance['ID'];
        }

        $start = $response->getResponseData()->getPagination()->getNextItem();
    } while ($start !== null);

    if ($workflowIds === []) {
        echo "No workflows found\n";
        exit;
    }

    echo "Found workflows: " . count($workflowIds) . "\n";
    foreach ($workflowIds as $workflowId) {
        echo "Workflow to delete: {$workflowId}\n";
    }

    if (!$confirmed) {
        echo "Review the list and rerun the example with the --confirm argument to delete the workflows\n";
        exit;
    }

    foreach ($workflowIds as $workflowId) {
        $isKilled = $b24->getBizProcScope()->workflow()->kill($workflowId)->isSuccess();
        echo $isKilled
            ? "Workflow {$workflowId} deleted\n"
            : "Error deleting workflow {$workflowId}\n";
    }
    ```

- Python

    ```python
    import re
    import sys

    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    user_date_input = input("Enter the date in dd.mm.yyyy format: ")
    if not re.match(r"^\d{2}\.\d{2}\.\d{4}$", user_date_input):
        raise ValueError("Enter the date in dd.mm.yyyy format")

    day, month, year = user_date_input.split(".")
    iso_date = f"{year}-{month}-{day}T00:00:00Z"
    confirmed = "--confirm" in sys.argv

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )
    client = Client(token)

    start = None
    workflow_ids = []
    while True:
        params = {
            "select": ["ID", "STARTED"],
            "filter": {"<STARTED": iso_date},
        }
        if start is not None:
            params["start"] = start

        response = client.bizproc.workflow.instances(**params).response
        workflow_ids.extend(instance["ID"] for instance in response.result or [])

        if response.next is None:
            break
        start = response.next

    if not workflow_ids:
        print("No workflows found")
        sys.exit(0)

    print(f"Found workflows: {len(workflow_ids)}")
    for workflow_id in workflow_ids:
        print(f"Workflow to delete: {workflow_id}")

    if not confirmed:
        print("Review the list and rerun the example with the --confirm argument to delete the workflows")
        sys.exit(0)

    for workflow_id in workflow_ids:
        try:
            token.call_method("bizproc.workflow.kill", {"ID": workflow_id})
        except BitrixAPIError as error:
            print(f"Error deleting workflow {workflow_id}: {error}")
        else:
            print(f"Workflow {workflow_id} deleted")
    ```

- Go

    ```go
    // Setup in an empty directory:
    //  go mod init example && go get github.com/bitrix24/b24gosdk
    //
    // Run:
    //  B24_WEBHOOK_URL="https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/" go run main.go 01.01.2025
    //  B24_WEBHOOK_URL="https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/" go run main.go 01.01.2025 --confirm

    package main

    import (
        "context"
        "encoding/json"
        "fmt"
        "os"
        "strings"

        b24 "github.com/bitrix24/b24gosdk"
    )

    type workflowInstance struct {
        ID      string `json:"ID"`
        Started string `json:"STARTED"`
    }

    func main() {
        if err := run(); err != nil {
            fmt.Fprintf(os.Stderr, "%v\n", err)
            os.Exit(1)
        }
    }

    func run() error {
        if len(os.Args) < 2 {
            return fmt.Errorf("pass the date in dd.mm.yyyy format")
        }

        parts := strings.Split(os.Args[1], ".")
        if len(parts) != 3 || len(parts[0]) != 2 || len(parts[1]) != 2 || len(parts[2]) != 4 {
            return fmt.Errorf("pass the date in dd.mm.yyyy format")
        }

        isoDate := fmt.Sprintf("%s-%s-%sT00:00:00Z", parts[2], parts[1], parts[0])
        confirmed := false
        for _, arg := range os.Args[2:] {
            if arg == "--confirm" {
                confirmed = true
            }
        }

        ctx := context.Background()
        core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

        workflowIDs, err := getWorkflowIDs(ctx, core, isoDate)
        if err != nil {
            return err
        }

        if len(workflowIDs) == 0 {
            fmt.Println("No workflows found")
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

    func getWorkflowIDs(ctx context.Context, core *b24.Core, isoDate string) ([]string, error) {
        workflowIDs := []string{}

        for start := 0; ; start += 50 {
            res, err := core.Call(ctx, "bizproc.workflow.instances", b24.Params{
                "select": []string{"ID", "STARTED"},
                "filter": b24.Params{"<STARTED": isoDate},
                "start":  start,
            }, b24.WithIdempotent())
            if err != nil {
                return nil, fmt.Errorf("bizproc.workflow.instances: %w", err)
            }

            var instances []workflowInstance
            if err := json.Unmarshal(res.Result, &instances); err != nil {
                return nil, fmt.Errorf("decode bizproc.workflow.instances: %w", err)
            }

            for _, instance := range instances {
                workflowIDs = append(workflowIDs, instance.ID)
            }

            if len(instances) < 50 {
                break
            }
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

In the interface, open the list of running business processes and verify that the deleted processes are no longer present.

Through REST, repeat the [bizproc.workflow.instances](../../api-reference/bizproc/bizproc-workflow-instances.md) request with the same filter and verify that the deleted `ID` values are not returned.

{% list tabs %}

- JS

    ```js
    const checkResponse = await $b24.actions.v2.call.make({
        method: 'bizproc.workflow.instances',
        params: {
            select: ['ID'],
            filter: { '<STARTED': '2025-01-01T00:00:00Z' },
        },
        requestId: 'workflow-instances-check',
    })

    console.log(checkResponse.getData().result.map((instance) => instance.ID))
    ```

- PHP

    ```php
    $checkResponse = $b24->core->call('bizproc.workflow.instances', [
        'select' => ['ID'],
        'filter' => ['<STARTED' => '2025-01-01T00:00:00Z'],
    ]);

    foreach ($checkResponse->getResponseData()->getResult() as $instance) {
        echo $instance['ID'] . PHP_EOL;
    }
    ```

- Python

    ```python
    check_result = client.bizproc.workflow.instances(
        select=["ID"],
        filter={"<STARTED": "2025-01-01T00:00:00Z"},
    ).response.result

    print([instance["ID"] for instance in check_result])
    ```

- Go

    ```go
    res, err := core.Call(ctx, "bizproc.workflow.instances", b24.Params{
        "select": []string{"ID"},
        "filter": b24.Params{"<STARTED": "2025-01-01T00:00:00Z"},
    }, b24.WithIdempotent())
    if err != nil {
        log.Fatal(err)
    }

    var instances []struct {
        ID string `json:"ID"`
    }
    if err := json.Unmarshal(res.Result, &instances); err != nil {
        log.Fatal(err)
    }

    for _, instance := range instances {
        log.Println(instance.ID)
    }
    ```

{% endlist %}

The scenario is complete if the response no longer contains the `ID` values that were passed to `bizproc.workflow.kill`.

## Errors and Diagnostics

If a method returns an error, verify the request data.

#|
|| **Error code or text** | **Cause and action** ||
|| `ACCESS_DENIED` | The method was not called by an administrator or the webhook does not have the `bizproc` scope ||
|| `ERROR_WRONG_WORKFLOW_ID` | `ID` is empty or has a non-string value ||
|| Empty `result` array in step 1 | There are no running processes that match the `<STARTED` filter ||
|| Errors with large volumes of data | Check the paging logic and the [Performance](../../settings/performance/index.md) recommendations ||
|#

Repeat the scenario from the step where the error occurred. If the error happens while deleting one process, verify its `ID` and continue processing the remaining processes.

## What to Consider

- `bizproc.workflow.kill` deletes the process together with its process data
- `bizproc.workflow.terminate` stops the process and keeps the record of its launch
- the business process `ID` is a string like `660e559f34af10.95144732`; do not convert it to a number
- `bizproc.workflow.instances` returns 50 records per request, so mass processing requires pagination

## Continue Learning

- [Get the list of running business processes](../../api-reference/bizproc/bizproc-workflow-instances.md)
- [Delete a running process](../../api-reference/bizproc/bizproc-workflow-kill.md)
- [Terminate an active business process](../../api-reference/bizproc/bizproc-workflow-terminate.md)
