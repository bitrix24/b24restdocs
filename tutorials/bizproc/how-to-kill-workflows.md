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

- Go

    ```go
    // ACTIVE: 0 selects only dismissed employees. Without this parameter, the search covers
    // all employees regardless of status. The first and last names are set by the constants
    // departedName and departedLastName at the top of the file; empty values are not put into
    // the filter — an empty string matches nobody.
    filter := b24.Params{"ACTIVE": 0}
    if departedName != "" {
    	filter["NAME"] = departedName
    }
    if departedLastName != "" {
    	filter["LAST_NAME"] = departedLastName
    }

    res, err := core.Call(ctx, "user.get", b24.Params{"filter": filter}, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("user.get: %w", err)
    }

    // user.get responds in UPPER_SNAKE and sends the ID AS A STRING ("29"):
    // b24.ID parses both a number and a string containing a number.
    var users []struct {
    	ID       b24.ID `json:"ID"`
    	Name     string `json:"NAME"`
    	LastName string `json:"LAST_NAME"`
    }
    if err := json.Unmarshal(res.Result, &users); err != nil {
    	return fmt.Errorf("parse employees: %w", err)
    }
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

- Go

    ```go
    // STATUS: 0 — only incomplete checklist items.
    res, err := core.Call(ctx, "bizproc.task.list", b24.Params{
    	"filter": b24.Params{"USER_ID": userID, "STATUS": 0},
    }, b24.WithIdempotent())
    if err != nil {
    	// bizproc.* is available only to the portal administrator and only on
    	// paid plans. The code is compared with errors.Is rather than as a string:
    	// a typo in the literal would compile and silently take a different branch.
    	if errors.Is(err, b24.ErrMethodNotFound) {
    		return fmt.Errorf("the business processes module is not available on this portal: %w", err)
    	}
    	return fmt.Errorf("bizproc.task.list: %w", err)
    }

    // WORKFLOW_ID is NOT a number: "67e3db8e581121.72266518". Parsing it into
    // int destroys the value, so the field stays a string from the list up to
    // the termination command.
    var tasks []struct {
    	ID           b24.ID `json:"ID"`
    	WorkflowID   string `json:"WORKFLOW_ID"`
    	Name         string `json:"NAME"`
    	DocumentName string `json:"DOCUMENT_NAME"`
    }
    if err := json.Unmarshal(res.Result, &tasks); err != nil {
    	return fmt.Errorf("parse workflow tasks: %w", err)
    }
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

- Go

    ```go
    res, err := core.Call(ctx, "bizproc.workflow.kill", b24.Params{"ID": workflowID})
    if err != nil {
    	// An already finished workflow cannot be terminated — this is not a failure
    	// of the scenario; the remaining workflows still have to be terminated.
    	fmt.Fprintf(os.Stderr, "workflow %s: %v\n", workflowID, err)
    	continue
    }

    // The response is a bare boolean rather than an object.
    var killed bool
    if err := json.Unmarshal(res.Result, &killed); err != nil {
    	return fmt.Errorf("parse the bizproc.workflow.kill response: %w", err)
    }
    fmt.Printf("workflow %s terminated: %v\n", workflowID, killed)
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

- Go

    ```go
    // Setup in an empty directory — go get will not work without go mod init:
    //
    //	go mod init example && go get github.com/bitrix24/b24gosdk
    //
    // Run:
    //
    //	export B24_WEBHOOK_URL='https://your-portal.bitrix24.com/rest/1/token/' && go run .
    //
    // The example is self-contained and safe: it creates ITS OWN deal, terminates
    // only the workflows that started on it, and deletes the deal afterwards.
    // A dismissed employee cannot be created and "dismissed", so it performs steps 1 and 2
    // for real and shows what it found, while it terminates only its own.
    // It runs on any portal, nothing needs to be edited.
    package main

    import (
    	"context"
    	"encoding/json"
    	"errors"
    	"fmt"
    	"log"
    	"os"
    	"time"

    	b24 "github.com/bitrix24/b24gosdk"
    )

    // The first and last name of the dismissed employee for step 1. Empty values mean
    // "all dismissed employees": a specific person cannot be found on someone else's portal, and the example
    // must run everywhere without edits.
    const (
    	departedName     = ""
    	departedLastName = ""
    )

    func main() {
    	if err := run(context.Background()); err != nil {
    		log.Fatal(err)
    	}
    }

    func run(ctx context.Context) error {
    	// The webhook path is a secret, so it comes from the environment, not from the code.
    	core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

    	// --- setup: our own deal and the workflows started on it

    	dealID, err := addDeal(ctx, core)
    	if err != nil {
    		return err
    	}
    	// Deleting a deal also removes the unfinished workflows on it.
    	defer del(ctx, core, "crm.deal.delete", b24.Params{"id": dealID})

    	// Workflows do not start instantly.
    	time.Sleep(3 * time.Second)

    	mine, err := workflowsOfDeal(ctx, core, dealID)
    	if err != nil {
    		return err
    	}
    	fmt.Printf("workflows started on deal %d: %d\n", dealID, len(mine))

    	// --- step 1: the ID of the dismissed employee
    	// ACTIVE: 0 selects only dismissed employees. Without this parameter, the search covers
    	// all employees regardless of status. The first and last names are set by the constants
    	// departedName and departedLastName at the top of the file; empty values are not put into
    	// the filter — an empty string matches nobody.
    	filter := b24.Params{"ACTIVE": 0}
    	if departedName != "" {
    		filter["NAME"] = departedName
    	}
    	if departedLastName != "" {
    		filter["LAST_NAME"] = departedLastName
    	}

    	res, err := core.Call(ctx, "user.get", b24.Params{"filter": filter}, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("user.get: %w", err)
    	}

    	// user.get responds in UPPER_SNAKE and sends the ID AS A STRING ("29"):
    	// b24.ID parses both a number and a string containing a number.
    	var users []struct {
    		ID       b24.ID `json:"ID"`
    		Name     string `json:"NAME"`
    		LastName string `json:"LAST_NAME"`
    	}
    	if err := json.Unmarshal(res.Result, &users); err != nil {
    		return fmt.Errorf("parse employees: %w", err)
    	}
    	fmt.Printf("dismissed employees found: %d\n", len(users))

    	// --- step 2: the workflow tasks the employee is responsible for

    	// If there are no dismissed employees, request the tasks of the current user: the call itself
    	// does not change because of it, and the scenario remains runnable on any portal.
    	targets := make([]b24.ID, 0, len(users))
    	for _, u := range users {
    		targets = append(targets, u.ID)
    	}
    	if len(targets) == 0 {
    		me, err := currentUser(ctx, core)
    		if err != nil {
    			return err
    		}
    		targets = append(targets, me)
    	}

    	for _, userID := range targets {
    		// STATUS: 0 — only incomplete checklist items.
    		res, err := core.Call(ctx, "bizproc.task.list", b24.Params{
    			"filter": b24.Params{"USER_ID": userID, "STATUS": 0},
    		}, b24.WithIdempotent())
    		if err != nil {
    			// bizproc.* is available only to the portal administrator and only on
    			// paid plans. The code is compared with errors.Is rather than as a string:
    			// a typo in the literal would compile and silently take a different branch.
    			if errors.Is(err, b24.ErrMethodNotFound) {
    				return fmt.Errorf("the business processes module is not available on this portal: %w", err)
    			}
    			return fmt.Errorf("bizproc.task.list: %w", err)
    		}

    		// WORKFLOW_ID is NOT a number: "67e3db8e581121.72266518". Parsing it into
    		// int destroys the value, so the field stays a string from the list up to
    		// the termination command.
    		var tasks []struct {
    			ID           b24.ID `json:"ID"`
    			WorkflowID   string `json:"WORKFLOW_ID"`
    			Name         string `json:"NAME"`
    			DocumentName string `json:"DOCUMENT_NAME"`
    		}
    		if err := json.Unmarshal(res.Result, &tasks); err != nil {
    			return fmt.Errorf("parse workflow tasks: %w", err)
    		}
    		fmt.Printf("employee %d, incomplete workflow tasks: %d\n", userID, len(tasks))
    		for _, t := range tasks {
    			fmt.Printf("  workflow task %d %q, workflow %s (%s)\n",
    				t.ID, t.Name, t.WorkflowID, t.DocumentName)
    		}
    	}

    	// --- step 3: terminate the workflows

    	// On a production portal, the WORKFLOW_ID from step 2 is substituted here. The example
    	// is limited to the workflows of ITS OWN deal: it has no right to terminate
    	// someone else's workflow on your portal.
    	if len(mine) == 0 {
    		fmt.Println("no workflows started on the example deal — there is nothing to terminate")
    		return nil
    	}

    	for _, workflowID := range mine {
    		res, err := core.Call(ctx, "bizproc.workflow.kill", b24.Params{"ID": workflowID})
    		if err != nil {
    			// An already finished workflow cannot be terminated — this is not a failure
    			// of the scenario; the remaining workflows still have to be terminated.
    			fmt.Fprintf(os.Stderr, "workflow %s: %v\n", workflowID, err)
    			continue
    		}

    		// The response is a bare boolean rather than an object.
    		var killed bool
    		if err := json.Unmarshal(res.Result, &killed); err != nil {
    			return fmt.Errorf("parse the bizproc.workflow.kill response: %w", err)
    		}
    		fmt.Printf("workflow %s terminated: %v\n", workflowID, killed)
    	}

    	// bizproc.workflow.terminate stops the workflow but RETAINS the record of
    	// it; kill deletes the workflow together with its data. They are called the same way.
    	return nil
    }

    // --- helpers: data setup and cleanup

    func addDeal(ctx context.Context, core *b24.Core) (b24.ID, error) {
    	res, err := core.Call(ctx, "crm.deal.add", b24.Params{
    		"fields": b24.Params{"TITLE": "Deal for the b24gosdk example"},
    	})
    	if err != nil {
    		return 0, fmt.Errorf("crm.deal.add: %w", err)
    	}
    	var id b24.ID
    	return id, json.Unmarshal(res.Result, &id)
    }

    // workflowsOfDeal returns the IDs of the workflows started on the deal.
    func workflowsOfDeal(ctx context.Context, core *b24.Core, dealID b24.ID) ([]string, error) {
    	// bizproc.workflow.instances accepts parameters in UPPERCASE. SELECT
    	// here is not decoration: by default the method returns only ID, MODIFIED, and
    	// OWNED_UNTIL, while a missing field silently unmarshals into a zero value.
    	res, err := core.Call(ctx, "bizproc.workflow.instances", b24.Params{
    		"SELECT": []string{"ID", "TEMPLATE_ID", "DOCUMENT_ID", "STARTED"},
    		"FILTER": b24.Params{"DOCUMENT_ID": fmt.Sprintf("DEAL_%d", dealID)},
    	}, b24.WithIdempotent())
    	if err != nil {
    		if errors.Is(err, b24.ErrAccessDenied) {
    			return nil, fmt.Errorf("bizproc.* is available only to the portal administrator: %w", err)
    		}
    		if errors.Is(err, b24.ErrMethodNotFound) {
    			return nil, fmt.Errorf("the business processes module is not available on this portal: %w", err)
    		}
    		return nil, fmt.Errorf("bizproc.workflow.instances: %w", err)
    	}
    	var instances []struct {
    		ID string `json:"ID"`
    	}
    	if err := json.Unmarshal(res.Result, &instances); err != nil {
    		return nil, fmt.Errorf("parse workflows: %w", err)
    	}
    	ids := make([]string, 0, len(instances))
    	for _, i := range instances {
    		if i.ID != "" {
    			ids = append(ids, i.ID)
    		}
    	}
    	return ids, nil
    }

    func currentUser(ctx context.Context, core *b24.Core) (b24.ID, error) {
    	res, err := core.Call(ctx, "user.current", nil, b24.WithIdempotent())
    	if err != nil {
    		return 0, fmt.Errorf("user.current: %w", err)
    	}
    	var u struct {
    		ID b24.ID `json:"ID"`
    	}
    	if err := json.Unmarshal(res.Result, &u); err != nil {
    		return 0, err
    	}
    	return u.ID, nil
    }

    // del removes what was created. A cleanup error is printed but not returned: it must not
    // mask the real error of the scenario.
    func del(ctx context.Context, core *b24.Core, method string, params b24.Params) {
    	if _, err := core.Call(ctx, method, params); err != nil {
    		fmt.Fprintf(os.Stderr, "cleanup, %s: %v
", method, err)
    	}
    }
    ```

{% endlist %}
