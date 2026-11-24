# How to Mass Terminate Workflows with Date Filtering

> Scope: [`bizproc`](../../api-reference/scopes/permissions.md)
> 
> Who can execute methods: administrator

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Bitrix24 can retain running workflows that are no longer needed: they are stuck, waiting for outdated data, or were launched before a process change. You can find them by launch date and remove them using the `bizproc.workflow.kill` method.

The `bizproc.workflow.kill` method deletes the workflow together with its data. If you need to stop execution but keep the launch record, use [bizproc.workflow.terminate](../../api-reference/bizproc/bizproc-workflow-terminate.md). Both methods accept the same workflow identifier.

The scenario consists of two steps.

1. Retrieve the list of workflows using the [bizproc.workflow.instances](../../api-reference/bizproc/bizproc-workflow-instances.md) method
2. Remove the selected workflows using the [bizproc.workflow.kill](../../api-reference/bizproc/bizproc-workflow-kill.md) method

## What You Need Before You Start

- an administrator inbound webhook with the `bizproc` scope: the examples initialize the SDK using a webhook
- a date before which running workflows should be found. The examples use `2025-01-01T00:00:00Z`
- to stop workflows instead of deleting them, replace `bizproc.workflow.kill` with `bizproc.workflow.terminate` in the examples. In PHP, replace the typed `workflow()->kill(...)` call with `$b24->core->call('bizproc.workflow.terminate', ['ID' => ...])`: use `$workflowIds[0]` in step 2 and `$workflowId` in the full example

Before deleting, make sure to verify the found `ID` values. The full example displays the list and asks for confirmation during the same run: only the `ID` values from the displayed list are deleted, with no new search before deletion.

{% include [Note on examples](../../_includes/examples.md) %}

## 1. Retrieve the List of Processes {#workflow-id}

Use the [bizproc.workflow.instances](../../api-reference/bizproc/bizproc-workflow-instances.md) method with the following parameters:

- `filter[<STARTED]` — launch date. The `<` prefix selects workflows started before the specified time
- `select` — the fields required by the scenario. It is enough to get `ID` and `STARTED`

If you change the filter, check the field name and operation prefix: the method skips unknown fields or operations without an error, so the request will return workflows without a date restriction. The examples in this step retrieve the first page, up to 50 workflows. For mass processing, use the [Full Example](#full-example).

{% list tabs %}

- JS
  
    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_WEBHOOK_URL)
    // B24_WEBHOOK_URL = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

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

The examples below delete the first workflow in the list without asking for confirmation. Check its `ID` and launch date first. To review and confirm the entire list, use the [Full Example](#full-example).

{% list tabs %}

- JS

    ```js
    const killResponse = await $b24.actions.v2.call.make({
        method: 'bizproc.workflow.kill',
        params: { ID: workflowIds[0] },
        requestId: 'workflow-kill',
    })

    const isKilled = killResponse.getData().result
    ```

- Python

    ```python
    is_killed = client.bizproc.workflow.kill(workflow_ids[0]).response.result
    ```

- PHP

    ```php
    $isKilled = $b24->getBizProcScope()->workflow()
        ->kill($workflowIds[0])
        ->isSuccess();
    ```

- Go

    ```go
    killRes, killErr := core.Call(ctx, "bizproc.workflow.kill", b24.Params{
        "ID": workflowIDs[0],
    })
    if killErr != nil {
        log.Fatal(killErr)
    }

    var isKilled bool
    if err := json.Unmarshal(killRes.Result, &isKilled); err != nil {
        log.Fatal(err)
    }
    log.Println(isKilled)
    ```

{% endlist %}

The successful response contains `true`. The method does not return `false`: if a workflow cannot be deleted, it returns an error. See [Errors and Diagnostics](#errors) for details on handling it.

```json
{
    "result": true
}
```

## Code Example {#full-example}

Running workflows may finish during pagination, so the list may not include all matching workflows. After processing, repeat the search to retrieve a new list. The example first displays all found workflows with their launch dates for review. Check the identifiers and dates, and confirm deletion only if all workflows in the list can be deleted. To delete them, enter `yes` at the confirmation prompt. Any other response cancels deletion.

Install the SDK for your chosen language: `npm install @bitrix24/b24jssdk`, `pip install b24pysdk`, `composer require bitrix24/b24phpsdk:"^3.0"`, or `go get github.com/bitrix24/b24gosdk`. Save the example to a file and pass the webhook URL through environment variables rather than in the code: `B24_WEBHOOK_URL` for JS, PHP, and Go, and `B24_DOMAIN` and `B24_WEBHOOK_TOKEN` for Python. Run the JS example as an ES module, for example `node kill.mjs 01.01.2025`. It accepts the date as an argument, as does Go. Python and PHP prompt for the date at startup. The cutoff is 00:00 UTC on the specified date. For large volumes of data, consider the REST limits and the [Performance](../../settings/performance/index.md) recommendations.

{% list tabs %}

- JS

    ```js
    import readline from 'node:readline/promises'
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_WEBHOOK_URL)

    const [day, month, year] = (process.argv[2] || '').split('.')
    const isoDate = `${year}-${month}-${day}T00:00:00Z`
    const parsed = new Date(isoDate)
    if (Number.isNaN(parsed.getTime()) || parsed.toISOString().slice(0, 10) !== isoDate.slice(0, 10)) {
        throw new Error('Enter a valid date in dd.mm.yyyy format')
    }

    // The workflow ID is a string, so paginate using start rather than callList
    const instances = []
    for (let start = 0; ; start += 50) {
        const pageResponse = await $b24.actions.v2.call.make({
            method: 'bizproc.workflow.instances',
            params: {
                select: ['ID', 'STARTED'],
                filter: { '<STARTED': isoDate },
                order: { ID: 'ASC' },
                start,
            },
            requestId: `workflow-instances-${start}`,
        })

        if (!pageResponse.isSuccess) {
            throw new Error(pageResponse.getErrorMessages().join('; '))
        }

        const page = pageResponse.getData().result
        instances.push(...page)
        if (page.length < 50) {
            break
        }
    }

    const workflowIds = instances.map((instance) => instance.ID)

    if (!workflowIds.length) {
        console.log('No workflows found')
        $b24.destroy()
        process.exit(0)
    }

    console.log(`Found workflows: ${workflowIds.length}`)
    for (const instance of instances) {
        console.log(`Workflow to delete: ${instance.ID}, started: ${instance.STARTED}`)
    }

    const rl = readline.createInterface({ input: process.stdin, output: process.stdout })
    const answer = await rl.question(`Delete the workflows in the list (${workflowIds.length})? Enter yes: `)
    rl.close()

    if (answer.trim() !== 'yes') {
        console.log('Deletion canceled')
        $b24.destroy()
        process.exit(0)
    }

    for (const workflowId of workflowIds) {
        try {
            const killResponse = await $b24.actions.v2.call.make({
                method: 'bizproc.workflow.kill',
                params: { ID: workflowId },
                requestId: `workflow-kill-${workflowId}`,
            })

            console.log(killResponse.isSuccess
                ? `Workflow ${workflowId} deleted`
                : `Error deleting workflow ${workflowId}: ${killResponse.getErrorMessages().join('; ')}`)
        } catch (error) {
            console.log(`Error deleting workflow ${workflowId}: ${error.message}`)
        }
    }

    $b24.destroy()
    ```

- Python

    ```python
    import os
    import sys
    from datetime import datetime

    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    user_date_input = input("Enter the date in dd.mm.yyyy format: ")
    try:
        date = datetime.strptime(user_date_input, "%d.%m.%Y")
    except ValueError:
        raise SystemExit("Enter a valid date in dd.mm.yyyy format")

    iso_date = date.strftime("%Y-%m-%dT00:00:00Z")

    token = BitrixWebhook(
        domain=os.environ["B24_DOMAIN"],  # your-domain.bitrix24.com
        webhook_token=os.environ["B24_WEBHOOK_TOKEN"],  # user_id/webhook_key
    )
    client = Client(token)

    start = None
    workflow_ids = []
    started = {}
    while True:
        params = {
            "select": ["ID", "STARTED"],
            "filter": {"<STARTED": iso_date},
            "order": {"ID": "ASC"},
        }
        if start is not None:
            params["start"] = start

        response = client.bizproc.workflow.instances(**params).response
        for instance in response.result or []:
            workflow_ids.append(instance["ID"])
            started[instance["ID"]] = instance["STARTED"]

        if response.next is None:
            break
        start = response.next

    if not workflow_ids:
        print("No workflows found")
        sys.exit(0)

    print(f"Found workflows: {len(workflow_ids)}")
    for workflow_id in workflow_ids:
        print(f"Workflow to delete: {workflow_id}, started: {started[workflow_id]}")

    answer = input(f"Delete the workflows in the list ({len(workflow_ids)})? Enter yes: ")
    if answer.strip() != "yes":
        print("Deletion canceled")
        sys.exit(0)

    for workflow_id in workflow_ids:
        try:
            client.bizproc.workflow.kill(workflow_id).response
        except BitrixAPIError as error:
            print(f"Error deleting workflow {workflow_id}: {error}")
        else:
            print(f"Workflow {workflow_id} deleted")
    ```

- PHP

    ```php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Core\Exceptions\BaseException;
    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $b24 = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook(getenv('B24_WEBHOOK_URL'));

    $userDateInput = (string)readline('Enter the date in dd.mm.yyyy format: ');
    $date = DateTimeImmutable::createFromFormat('!d.m.Y', $userDateInput);
    if ($date === false || $date->format('d.m.Y') !== $userDateInput) {
        throw new InvalidArgumentException('Enter a valid date in dd.mm.yyyy format');
    }

    $isoDate = $date->format('Y-m-d') . 'T00:00:00Z';
    $workflowIds = [];
    $started = [];
    $start = 0;

    do {
        $response = $b24->core->call('bizproc.workflow.instances', [
            'select' => ['ID', 'STARTED'],
            'filter' => ['<STARTED' => $isoDate],
            'order' => ['ID' => 'ASC'],
            'start' => $start,
        ]);

        foreach ($response->getResponseData()->getResult() as $instance) {
            $workflowIds[] = $instance['ID'];
            $started[$instance['ID']] = $instance['STARTED'];
        }

        $start = $response->getResponseData()->getPagination()->getNextItem();
    } while ($start !== null);

    if ($workflowIds === []) {
        echo "No workflows found\n";
        exit;
    }

    echo "Found workflows: " . count($workflowIds) . "\n";
    foreach ($workflowIds as $workflowId) {
        echo "Workflow to delete: {$workflowId}, started: {$started[$workflowId]}\n";
    }

    $answer = readline('Delete the workflows in the list (' . count($workflowIds) . ')? Enter yes: ');
    if (trim((string)$answer) !== 'yes') {
        echo "Deletion canceled\n";
        exit;
    }

    foreach ($workflowIds as $workflowId) {
        try {
            $b24->getBizProcScope()->workflow()->kill($workflowId);
            echo "Workflow {$workflowId} deleted\n";
        } catch (BaseException $e) {
            echo "Error deleting workflow {$workflowId}: {$e->getMessage()}\n";
        }
    }
    ```

- Go

    ```go
    // Setup in an empty directory:
    //  go mod init example && go get github.com/bitrix24/b24gosdk
    //
    // Run:
    //  B24_WEBHOOK_URL="https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/" go run main.go 01.01.2025

    package main

    import (
        "bufio"
        "context"
        "encoding/json"
        "fmt"
        "os"
        "strings"
        "time"

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

        date, err := time.Parse("02.01.2006", os.Args[1])
        if err != nil {
            return fmt.Errorf("enter a valid date in dd.mm.yyyy format")
        }

        isoDate := date.Format("2006-01-02") + "T00:00:00Z"
        ctx := context.Background()
        core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

        workflows, err := getWorkflows(ctx, core, isoDate)
        if err != nil {
            return err
        }

        workflowIDs := make([]string, 0, len(workflows))
        for _, workflow := range workflows {
            workflowIDs = append(workflowIDs, workflow.ID)
        }

        if len(workflowIDs) == 0 {
            fmt.Println("No workflows found")
            return nil
        }

        fmt.Printf("Found workflows: %d\n", len(workflowIDs))
        for _, workflow := range workflows {
            fmt.Printf("Workflow to delete: %s, started: %s\n", workflow.ID, workflow.Started)
        }

        fmt.Printf("Delete the workflows in the list (%d)? Enter yes: ", len(workflowIDs))
        answer, _ := bufio.NewReader(os.Stdin).ReadString('\n')
        if strings.TrimSpace(answer) != "yes" {
            fmt.Println("Deletion canceled")
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

    func getWorkflows(ctx context.Context, core *b24.Core, isoDate string) ([]workflowInstance, error) {
        workflows := []workflowInstance{}

        for start := 0; ; start += 50 {
            res, err := core.Call(ctx, "bizproc.workflow.instances", b24.Params{
                "select": []string{"ID", "STARTED"},
                "filter": b24.Params{"<STARTED": isoDate},
                "order":  b24.Params{"ID": "ASC"},
                "start":  start,
            }, b24.WithIdempotent())
            if err != nil {
                return nil, fmt.Errorf("bizproc.workflow.instances: %w", err)
            }

            var instances []workflowInstance
            if err := json.Unmarshal(res.Result, &instances); err != nil {
                return nil, fmt.Errorf("decode bizproc.workflow.instances: %w", err)
            }

            workflows = append(workflows, instances...)

            if len(instances) < 50 {
                break
            }
        }

        return workflows, nil
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

Through REST, repeat the [bizproc.workflow.instances](../../api-reference/bizproc/bizproc-workflow-instances.md) request with the same filter and verify that the deleted `ID` values are not returned. The request below checks the first page. To check all pages, use the full example and decline deletion.

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

- Python

    ```python
    check_result = client.bizproc.workflow.instances(
        select=["ID"],
        filter={"<STARTED": "2025-01-01T00:00:00Z"},
    ).response.result

    print([instance["ID"] for instance in check_result])
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

- Go

    ```go
    checkRes, checkErr := core.Call(ctx, "bizproc.workflow.instances", b24.Params{
        "select": []string{"ID"},
        "filter": b24.Params{"<STARTED": "2025-01-01T00:00:00Z"},
    }, b24.WithIdempotent())
    if checkErr != nil {
        log.Fatal(checkErr)
    }

    var checkInstances []struct {
        ID string `json:"ID"`
    }
    if err := json.Unmarshal(checkRes.Result, &checkInstances); err != nil {
        log.Fatal(err)
    }

    for _, instance := range checkInstances {
        log.Println(instance.ID)
    }
    ```

{% endlist %}

The scenario is complete if the results across all pages no longer contain the `ID` values that were passed to `bizproc.workflow.kill`.

## Errors and Diagnostics {#errors}

If a method returns an error, verify the request data.

#|
|| **Error code or text** | **Cause and action** ||
|| `ACCESS_DENIED` | The method was called by a user without administrator permissions. Create a webhook as an administrator and repeat the request ||
|| `insufficient_scope` | The webhook or application does not have the `bizproc` scope. Add the scope in the webhook or application settings ||
|| `ERROR_WRONG_WORKFLOW_ID` | `ID` is empty or has a non-string value ||
|| Empty error code and `The business process is not found` | There is no workflow with this `ID`: it has already been deleted, for example during a repeated run of the scenario, or the `ID` was copied incorrectly. Skip this `ID` and continue processing the remaining workflows ||
|| Empty `result` array in step 1 | There are no running processes that match the `<STARTED` filter ||
|#

If the request fails due to a connection or authorization error, check Bitrix24 availability and the webhook URL, then repeat the search and review the list again before confirming.

Repeat the scenario from the step where the error occurred. If the error happens while deleting one process, verify its `ID` and continue processing the remaining processes.

## What to Consider

- `bizproc.workflow.instances` returns 50 records per request, so mass processing requires pagination. By default, the list is sorted by `MODIFIED`, which changes for running workflows, so the examples sort by `ID`. The `callList.make()` method in b24jssdk is unsuitable for this list: it converts `ID` to a number, while the workflow `ID` is a string

## Continue Learning

- [Get the list of running business processes](../../api-reference/bizproc/bizproc-workflow-instances.md)
- [Delete a running process](../../api-reference/bizproc/bizproc-workflow-kill.md)
- [Terminate an active business process](../../api-reference/bizproc/bizproc-workflow-terminate.md)
