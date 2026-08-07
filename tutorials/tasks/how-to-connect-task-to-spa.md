# How to Attach a Task to an SPA

> Scope: [`crm`, `task`](../../api-reference/scopes/permissions.md)
> 
> Who can execute the methods:
> - [crm.enum.ownertype](../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) — any user
> - [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) — any user with permission to read CRM object items
> - [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) and [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) — any user

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A task is linked to a CRM item via the "CRM items" field — `UF_CRM_TASK`. The field accepts values in [format](../../api-reference/crm/data-types.md#crm-binding-format) `{PREFIX}_{ID}`:

- `PREFIX` — the short symbolic code of the [CRM object type](../../api-reference/crm/data-types.md#object_type). It indicates what the link is being added to: a deal, a lead, or a specific SPA
- `ID` — the identifier of a specific item of this type

For example, `Tb1_29` is an item with `id`: `29` SPA with the short code `Tb1`.

Both parts of the value must be retrieved before creating the task. Therefore, the scenario consists of three steps.

1. Retrieve the `entityTypeId` and `SYMBOL_CODE_SHORT` of the SPA using the [crm.enum.ownertype](../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method

2. Retrieve the SPA item identifier using the [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) method with the `entityTypeId` parameter

3. Create a task using the [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) method, passing the value composed of `SYMBOL_CODE_SHORT` and `id` of the item into `UF_CRM_TASK`

As a result, you will have a task where the SPA item is specified in the "CRM items" field. Verify the link using the [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) method.

## Prepare the Data

To perform this example, you need:

- An incoming webhook with scopes `crm` and `task`
- A created SPA and at least one of its items
- The identifier of the user who will be assigned as the task assignee. This can be retrieved using the [user.get](../../api-reference/user/user-get.md) and [user.current](../../api-reference/user/user-current.md) methods
- The list of required task fields. If mandatory custom fields are configured on the portal, they must also be passed in `tasks.task.add` — check the list using the [tasks.task.getFields](../../api-reference/tasks/tasks-task-get-fields.md) method

The webhook executes requests with the permissions of the user who created it. Do not publish the secret webhook code in client-side code or repositories — store it in environment variables.

The SPA must be configured to allow binding to tasks. In the [crm.type.add](../../api-reference/crm/universal/user-defined-object-types/crm-type-add.md) or [crm.type.update](../../api-reference/crm/universal/user-defined-object-types/crm-type-update.md) methods, you must pass two configurations simultaneously:

- `isUseInUserfieldEnabled`: `Y` — [allows using the SPA in custom fields](../../api-reference/crm/universal/user-defined-object-types/index.md)
- `linkedUserFields`: `{"TASKS_TASK|UF_CRM_TASK": "true"}` — adds the SPA specifically to the task field. The default value is an empty object

The `isUseInUserfieldEnabled` option alone is not enough: without `linkedUserFields`, the SPA will not appear in the list of types available to the `UF_CRM_TASK` field, and the link will not be retained.

For server-side JS examples with `B24Hook`, Node.js 18, 20, 22, or newer is required; for new projects, 22 or newer is required. B24JsSDK is an ES module: save the code in a file `.mjs` or add `"type": "module"` to `package.json`.

For examples with b24pysdk, Python 3.9 or newer is required.

## 1. Retrieve SPA Identifiers {#SPA-ids}

To retrieve an SPA identifier, use the [crm.enum.ownertype](../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method. The method is called without parameters and returns standard CRM object types and SPAs.

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const ownerTypeResponse = await $b24.actions.v2.call.make({
        method: 'crm.enum.ownertype',
        params: {},
        requestId: 'crm-enum-ownertype'
    })

    if (!ownerTypeResponse.isSuccess) {
        throw new Error(ownerTypeResponse.getErrorMessages().join('; '))
    }

    const ownerTypes = ownerTypeResponse.getData().result
    ```

- PHP

    ```php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    $result = $serviceBuilder->getCRMScope()->enum()->ownerType()->getItems();
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

    result = client.crm.enum.ownertype().response.result
    ```

- Go

    ```go
    // The method is called without parameters and returns both the preset types
    // of CRM objects and smart processes.
    res, err := core.Call(ctx, "crm.enum.ownertype", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.enum.ownertype: %w", err)
    }

    var ownerTypes []struct {
    	ID              int    `json:"ID"`
    	Name            string `json:"NAME"`
    	SymbolCode      string `json:"SYMBOL_CODE"`
    	SymbolCodeShort string `json:"SYMBOL_CODE_SHORT"`
    }
    if err := json.Unmarshal(res.Result, &ownerTypes); err != nil {
    	return fmt.Errorf("parse object types: %w", err)
    }

    // Look up your smart process by title and remember TWO values: ID is
    // entityTypeId to look up the item, SYMBOL_CODE_SHORT is the first half
    // of the binding value.
    var entityTypeIDFound int
    var symbolCodeShort string
    for _, t := range ownerTypes {
    	if t.Name == spaTitle {
    		entityTypeIDFound, symbolCodeShort = t.ID, t.SymbolCodeShort
    		break
    	}
    }
    if symbolCodeShort == "" {
    	return fmt.Errorf("smart process %q not found in crm.enum.ownertype", spaTitle)
    }
    ```

{% endlist %}

For each object type, the method returns four fields:

- `ID` — the numeric identifier of the `entityTypeId` type. This will be needed in the next step to find the SPA item
- `NAME` — the type name. Use this to find the required SPA in the list
- `SYMBOL_CODE` — the symbolic code of the type
- `SYMBOL_CODE_SHORT` — the short symbolic code. This is the first part of the binding value that we will pass to the task

Abbreviated response:

```json
{
    "result": [
        {
            "ID": 1,
            "NAME": "Lead",
            "SYMBOL_CODE": "LEAD",
            "SYMBOL_CODE_SHORT": "L"
        },
        {
            "ID": 2,
            "NAME": "Deal",
            "SYMBOL_CODE": "DEAL",
            "SYMBOL_CODE_SHORT": "D"
        },
        {
            "ID": 3,
            "NAME": "Contact",
            "SYMBOL_CODE": "CONTACT",
            "SYMBOL_CODE_SHORT": "C"
        },
        {
            "ID": 4,
            "NAME": "Company",
            "SYMBOL_CODE": "COMPANY",
            "SYMBOL_CODE_SHORT": "CO"
        },
        {
            "ID": 5,
            "NAME": "Invoice (old version)",
            "SYMBOL_CODE": "INVOICE",
            "SYMBOL_CODE_SHORT": "I"
        },
        {
            "ID": 31,
            "NAME": "Invoice",
            "SYMBOL_CODE": "SMART_INVOICE",
            "SYMBOL_CODE_SHORT": "SI"
        },
        {
            "ID": 7,
            "NAME": "Quotation",
            "SYMBOL_CODE": "QUOTE",
            "SYMBOL_CODE_SHORT": "Q"
        },
        {
            "ID": 8,
            "NAME": "Billing details",
            "SYMBOL_CODE": "REQUISITE",
            "SYMBOL_CODE_SHORT": "RQ"
        },
        {
            "ID": 36,
            "NAME": "Document",
            "SYMBOL_CODE": "SMART_DOCUMENT",
            "SYMBOL_CODE_SHORT": "DO"
        },
        {
            "ID": 39,
            "NAME": "Company document",
            "SYMBOL_CODE": "SMART_B2E_DOC",
            "SYMBOL_CODE_SHORT": "SBD"
        },
        {
            "ID": 177,
            "NAME": "Equipment procurement",
            "SYMBOL_CODE": "DYNAMIC_177",
            "SYMBOL_CODE_SHORT": "Tb1"
        },
        {
            "ID": 156,
            "NAME": "Procurement",
            "SYMBOL_CODE": "DYNAMIC_156",
            "SYMBOL_CODE_SHORT": "T9c"
        },
        ...
    ]
}
```

As a result, we have obtained CRM object types with identifiers. Next, we work with the "Equipment Procurement" SPA and save two of its values: `ID`: `177` to search for the item and `SYMBOL_CODE_SHORT`: `Tb1` for the binding.

## 2. Retrieve SPA Item ID {#element-id}

To retrieve the SPA item ID, use the [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) method with the following parameters:

-  `entityTypeId` — `177`, where the value equals `ID` from the previous method's result

-  `filter[title]` — specify the item name for the search

-  `select` — a list of returned fields. For the scenario, `id` and `title` are sufficient

{% list tabs %}

- JS
  
    ```javascript
    const itemResponse = await $b24.actions.v2.call.make({
        method: 'crm.item.list',
        params: {
            entityTypeId: 177, // ID from crm.enum.ownertype result
            select: [
                'id', // selectable fields
                'title',
            ],
            filter: {
                'title': 'Washing machine', // element name
            },
        },
        requestId: 'crm-item-list'
    })

    if (!itemResponse.isSuccess) {
        throw new Error(itemResponse.getErrorMessages().join('; '))
    }

    const items = itemResponse.getData().result.items
    ```

- PHP
  
    ```php
    $result = $serviceBuilder->getCRMScope()->item()->list(
        177, // ID from crm.enum.ownertype result
        [], // sorting
        [
            'title' => 'Washing machine', // element name
        ],
        [
            'id', // selectable fields
            'title',
        ]
    )->getItems();
    ```

- Python

    ```python
    result = client.crm.item.list(
        entity_type_id=177,
        select=["id", "title"],
        filter={
            "title": "Washing machine",
        },
    ).response.result
    ```

- Go

    ```go
    res, err = core.Call(ctx, "crm.item.list", b24.Params{
    	"entityTypeId": entityTypeIDFound,
    	"select":       []string{"id", "title"},
    	"filter":       b24.Params{"title": itemTitle},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.item.list: %w", err)
    }

    // The method wraps the response in an object with the items key. Identical titles
    // are not forbidden, so the response is a list even with an exact filter.
    var list struct {
    	Items []struct {
    		ID    int    `json:"id"`
    		Title string `json:"title"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &list); err != nil {
    	return fmt.Errorf("parse items: %w", err)
    }
    if len(list.Items) == 0 {
    	return fmt.Errorf("item %q not found", itemTitle)
    }
    ```

{% endlist %}

As a result, we obtained `id`: `29` of the SPA item — the second part of the binding value.

```json
{
    "result": {
        "items": [
            {
                "id": 29,
                "title": "Washing machine"
            }
        ]
    },
    "total": 1
}
```

If there are multiple items with the same name, the method will return all of them. Refine the filter or select the correct item by `id`.

## 3. Create a Task Bound to an SPA Item

Combine the binding value from the two retrieved parts. Place an underscore between them:

```text
SYMBOL_CODE_SHORT + "_" + id
Tb1 + "_" + 29 = Tb1_29
```

Construct the value from the method responses rather than using the ready-made string from the example. The short SPA code depends on `entityTypeId` and will be different on another portal.

To create a task, use the [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) method with the following parameters:

-  `UF_CRM_TASK` — an array of bindings to CRM items. Pass the combined `Tb1_29` value from [step 1](#SPA-ids) and [step 2](#element-id) into it

-  `TITLE` — the task name, a required field. The task will not be created without a name

-  `CREATED_BY` — the ID of the task creator. In the example, we do not pass it: the creator will be the user on whose behalf the request is being executed

-  `RESPONSIBLE_ID` — the ID of the task assignee, a required field. The task will not be created without an assignee

{% list tabs %}

- JS
  
    ```javascript
    const taskResponse = await $b24.actions.v2.call.make({
        method: 'tasks.task.add',
        params: {
            fields: {
                TITLE: 'task for test', // task name
                RESPONSIBLE_ID: 1, // assignee
                UF_CRM_TASK: [ // CRM items array
                    'Tb1_29'
                ]
            }
        },
        requestId: 'task-add'
    })

    if (!taskResponse.isSuccess) {
        throw new Error(taskResponse.getErrorMessages().join('; '))
    }

    const task = taskResponse.getData().result.task
    ```

- PHP
  
    ```php
    $result = $serviceBuilder->core->call(
        'tasks.task.add',
        [
            'fields' => [
                'TITLE' => 'task for test', // task name
                'RESPONSIBLE_ID' => 1, // assignee
                'UF_CRM_TASK' => [ // CRM items array
                    'Tb1_29'
                ]
            ]
        ]
    )->getResponseData()->getResult();
    ```

- Python

    ```python
    result = client.tasks.task.add(
        fields={
            "TITLE": "task for test",
            "RESPONSIBLE_ID": 1,
            "UF_CRM_TASK": [
                "Tb1_29",
            ],
        }
    ).response.result
    ```

- Go

    ```go
    // The binding value is assembled from the responses rather than from a ready-made string: the short
    // code depends on entityTypeId and will differ on another portal.
    binding := symbolCodeShort + "_" + strconv.Itoa(list.Items[0].ID)

    res, err = core.Call(ctx, "tasks.task.add", b24.Params{
    	"fields": b24.Params{
    		"TITLE":          "task for test",
    		"RESPONSIBLE_ID": userID,
    		// UF_CRM_TASK is always an array, even when there is a single binding.
    		"UF_CRM_TASK": []string{binding},
    	},
    })
    if err != nil {
    	return fmt.Errorf("tasks.task.add: %w", err)
    }

    // tasks.* wraps the response in an object with the task key, and the task ID
    // arrives AS A STRING ("3731"): b24.ID parses both a number and a string containing a number.
    var added struct {
    	Task struct {
    		ID    b24.ID `json:"id"`
    		Title string `json:"title"`
    	} `json:"task"`
    }
    if err := json.Unmarshal(res.Result, &added); err != nil {
    	return fmt.Errorf("parse the created task: %w", err)
    }
    ```

{% endlist %}

As a result, a task with ID `3731` was created. Save the identifier — we will use it to check the binding. Note: task methods return identifiers as strings — `"id": "3731"`. In `taskId` of the next call, the method will convert the value to a number, so a string containing a number will work. However, a non-numeric value will turn into `0`, and the method will return an error.

The method returns all task fields except for the binding to CRM items: the `ufCrmTask` field is not present in the response.

Abbreviated response:

```json
{
    "result": {
        "task": {
            "id": "3731",
            "title": "task for test",
            "status": "2",
            "createdBy": "1",
            "responsibleId": "1",
            "createdDate": "2025-01-20T14:30:58+02:00",
            "changedDate": "2025-01-20T14:30:58+02:00",
            "group": [],
            "checklist": [],
            ...
        }
    }
}
```

## Launch the Scenario

The script executes all three steps consecutively: finds the SPA by name, finds its item, collects the binding value, and creates a task. Replace the variable values with your own — in the examples above, this is the "Equipment Procurement" SPA, the "Washing Machine" item, and the task `task for test`.

{% list tabs %}

- JS
    
    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // Variables for user data input
    const smartProcessName = 'smart_process_name'; // Smart Process Name
    const itemName = 'element_name'; // Smart Process Element Name
    const responsibleId = 1; // Task assignee ID, number
    const taskTitle = 'task_name'; // Task Name

    // Function to create a task linked to a smart process element
    async function createTaskWithSmartProcess() {
        // Get entity and smart process type IDs
        const ownerTypeResponse = await $b24.actions.v2.call.make({
            method: 'crm.enum.ownertype',
            params: {},
            requestId: 'crm-enum-ownertype'
        });

        if (!ownerTypeResponse.isSuccess) {
            console.error('Error getting entity types:', ownerTypeResponse.getErrorMessages().join('; '));
            return;
        }

        // Searching for the required smart process
        const smartProcess = ownerTypeResponse.getData().result.find(function(process) {
            return process.NAME === smartProcessName;
        });

        if (!smartProcess) {
            console.error('Smart process not found');
            return;
        }

        const symbolCodeShort = smartProcess.SYMBOL_CODE_SHORT;

        // Searching for a smart process element using a name filter
        const itemResponse = await $b24.actions.v2.call.make({
            method: 'crm.item.list',
            params: {
                entityTypeId: smartProcess.ID,
                select: ['id', 'title'],
                filter: { 'title': itemName }
            },
            requestId: 'crm-item-list'
        });

        if (!itemResponse.isSuccess) {
            console.error('Error getting smart process elements:', itemResponse.getErrorMessages().join('; '));
            return;
        }

        if (itemResponse.getData().result.items.length === 0) {
            console.error('Smart process element not found');
            return;
        }

        const itemId = itemResponse.getData().result.items[0].id;

        // Creating a task
        const taskResponse = await $b24.actions.v2.call.make({
            method: 'tasks.task.add',
            params: {
                fields: {
                    TITLE: taskTitle, // Using the entered task name
                    RESPONSIBLE_ID: responsibleId, // Adding the assignee ID
                    UF_CRM_TASK: [symbolCodeShort + '_' + itemId]
                }
            },
            requestId: 'task-add'
        });

        if (!taskResponse.isSuccess) {
            console.error('Error creating task:', taskResponse.getErrorMessages().join('; '));
        } else {
            console.log('Task created successfully!', taskResponse.getData().result);
        }
    }

    // Calling the function to create a task
    await createTaskWithSmartProcess();

    $b24.destroy();
    ```

- PHP
  
    ```php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Bitrix24\SDK\Core\Exceptions\BaseException;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // Variables for user data input
    $smartProcessName = 'smart_process_name'; // Smart Process Name
    $itemName = 'element_name'; // Smart Process Element Name
    $responsibleId = 1; // Task assignee ID, number
    $taskTitle = 'task_name'; // Task Name

    // Function to create a task linked to a smart process element
    function createTaskWithSmartProcess($serviceBuilder, $smartProcessName, $itemName, $responsibleId, $taskTitle) {
        // Get entity and smart process type IDs
        try {
            $ownerTypes = $serviceBuilder->getCRMScope()->enum()->ownerType()->getItems();
        } catch (BaseException $e) {
            echo 'Error getting entity types: ' . $e->getMessage();
            return;
        }

        // Searching for the required smart process
        $smartProcess = null;
        foreach ($ownerTypes as $process) {
            if ($process->NAME === $smartProcessName) {
                $smartProcess = $process;
                break;
            }
        }

        if (!$smartProcess) {
            echo 'Smart process not found';
            return;
        }

        $symbolCodeShort = $smartProcess->SYMBOL_CODE_SHORT;

        // Searching for a smart process element using a name filter
        try {
            $items = $serviceBuilder->getCRMScope()->item()->list(
                $smartProcess->ID,
                [],
                ['title' => $itemName],
                ['id', 'title']
            )->getItems();
        } catch (BaseException $e) {
            echo 'Error getting smart process elements: ' . $e->getMessage();
            return;
        }

        if (count($items) === 0) {
            echo 'Smart process element not found';
            return;
        }

        $itemId = $items[0]->id;

        // Creating a task
        try {
            $taskResult = $serviceBuilder->core->call('tasks.task.add', [
                'fields' => [
                    'TITLE' => $taskTitle, // Using the entered task name
                    'RESPONSIBLE_ID' => $responsibleId, // Adding the assignee ID
                    'UF_CRM_TASK' => [$symbolCodeShort . '_' . $itemId]
                ]
            ])->getResponseData()->getResult();
        } catch (BaseException $e) {
            echo 'Error creating task: ' . $e->getMessage();
            return;
        }

        echo 'Task created successfully!';
        print_r($taskResult);
    }

    // Calling the function to create a task
    createTaskWithSmartProcess($serviceBuilder, $smartProcessName, $itemName, $responsibleId, $taskTitle);
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    smart_process_name = "smart_process_name"
    item_name = "element_name"
    responsible_id = 1
    task_title = "task_name"

    def create_task_with_smart_process(client, smart_process_name, item_name, responsible_id, task_title):
        try:
            result = client.crm.enum.ownertype().response.result
        except BitrixAPIError as error:
            print(f"Error getting entity types: {error}")
            return

        smart_process = None
        for process in result:
            if process["NAME"] == smart_process_name:
                smart_process = process
                break

        if smart_process is None:
            print("Smart process not found")
            return

        symbol_code_short = smart_process["SYMBOL_CODE_SHORT"]

        try:
            item_result = client.crm.item.list(
                entity_type_id=int(smart_process["ID"]),
                select=["id", "title"],
                filter={"title": item_name},
            ).response.result
        except BitrixAPIError as error:
            print(f"Error getting smart process elements: {error}")
            return

        if len(item_result["items"]) == 0:
            print("Smart process element not found")
            return

        item_id = item_result["items"][0]["id"]

        try:
            task_result = client.tasks.task.add(
                fields={
                    "TITLE": task_title,
                    "RESPONSIBLE_ID": responsible_id,
                    "UF_CRM_TASK": [f"{symbol_code_short}_{item_id}"],
                }
            ).response.result
        except BitrixAPIError as error:
            print(f"Error creating task: {error}")
        else:
            print("Task created successfully!")
            print(task_result)

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token=os.environ["B24_HOOK_TOKEN"],
        )
    )
    # B24_HOOK_TOKEN = 'user_id/webhook_key'

    create_task_with_smart_process(client, smart_process_name, item_name, responsible_id, task_title)
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
    // The example is self-contained: it creates a smart process allowed in the task field,
    // adds an item to it, assembles the binding value, creates a task,
    // verifies the binding and cleans up after itself. It runs on any portal, nothing
    // needs to be edited.
    package main

    import (
    	"context"
    	"encoding/json"
    	"errors"
    	"fmt"
    	"log"
    	"os"
    	"strconv"

    	b24 "github.com/bitrix24/b24gosdk"
    )

    const (
    	spaTitle  = "Equipment procurement (b24gosdk example)"
    	itemTitle = "Washing machine"
    )

    func main() {
    	if err := run(context.Background()); err != nil {
    		log.Fatal(err)
    	}
    }

    func run(ctx context.Context) error {
    	// The webhook path is a secret, so it comes from the environment, not from the code.
    	core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

    	// --- setup: a smart process that is allowed in the task field

    	entityTypeID, typeID, err := addType(ctx, core, spaTitle)
    	if err != nil {
    		return err
    	}
    	defer del(ctx, core, "crm.type.delete", b24.Params{"id": typeID})

    	itemID, err := addItem(ctx, core, entityTypeID, itemTitle)
    	if err != nil {
    		return err
    	}
    	defer del(ctx, core, "crm.item.delete", b24.Params{
    		"entityTypeId": entityTypeID, "id": itemID,
    	})

    	userID, err := currentUser(ctx, core)
    	if err != nil {
    		return err
    	}

    	// --- step 1: the smart process IDs
    	// The method is called without parameters and returns both the preset types
    	// of CRM objects and smart processes.
    	res, err := core.Call(ctx, "crm.enum.ownertype", nil, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("crm.enum.ownertype: %w", err)
    	}

    	var ownerTypes []struct {
    		ID              int    `json:"ID"`
    		Name            string `json:"NAME"`
    		SymbolCode      string `json:"SYMBOL_CODE"`
    		SymbolCodeShort string `json:"SYMBOL_CODE_SHORT"`
    	}
    	if err := json.Unmarshal(res.Result, &ownerTypes); err != nil {
    		return fmt.Errorf("parse object types: %w", err)
    	}

    	// Look up your smart process by title and remember TWO values: ID is
    	// entityTypeId to look up the item, SYMBOL_CODE_SHORT is the first half
    	// of the binding value.
    	var entityTypeIDFound int
    	var symbolCodeShort string
    	for _, t := range ownerTypes {
    		if t.Name == spaTitle {
    			entityTypeIDFound, symbolCodeShort = t.ID, t.SymbolCodeShort
    			break
    		}
    	}
    	if symbolCodeShort == "" {
    		return fmt.Errorf("smart process %q not found in crm.enum.ownertype", spaTitle)
    	}
    	fmt.Printf("smart process %q: entityTypeId=%d, short code %q\n",
    		spaTitle, entityTypeIDFound, symbolCodeShort)

    	// --- step 2: the item ID
    	res, err = core.Call(ctx, "crm.item.list", b24.Params{
    		"entityTypeId": entityTypeIDFound,
    		"select":       []string{"id", "title"},
    		"filter":       b24.Params{"title": itemTitle},
    	}, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("crm.item.list: %w", err)
    	}

    	// The method wraps the response in an object with the items key. Identical titles
    	// are not forbidden, so the response is a list even with an exact filter.
    	var list struct {
    		Items []struct {
    			ID    int    `json:"id"`
    			Title string `json:"title"`
    		} `json:"items"`
    	}
    	if err := json.Unmarshal(res.Result, &list); err != nil {
    		return fmt.Errorf("parse items: %w", err)
    	}
    	if len(list.Items) == 0 {
    		return fmt.Errorf("item %q not found", itemTitle)
    	}
    	fmt.Printf("item %q: id=%d\n", list.Items[0].Title, list.Items[0].ID)

    	// --- step 3: a task with a binding
    	// The binding value is assembled from the responses rather than from a ready-made string: the short
    	// code depends on entityTypeId and will differ on another portal.
    	binding := symbolCodeShort + "_" + strconv.Itoa(list.Items[0].ID)

    	res, err = core.Call(ctx, "tasks.task.add", b24.Params{
    		"fields": b24.Params{
    			"TITLE":          "task for test",
    			"RESPONSIBLE_ID": userID,
    			// UF_CRM_TASK is always an array, even when there is a single binding.
    			"UF_CRM_TASK": []string{binding},
    		},
    	})
    	if err != nil {
    		return fmt.Errorf("tasks.task.add: %w", err)
    	}

    	// tasks.* wraps the response in an object with the task key, and the task ID
    	// arrives AS A STRING ("3731"): b24.ID parses both a number and a string containing a number.
    	var added struct {
    		Task struct {
    			ID    b24.ID `json:"id"`
    			Title string `json:"title"`
    		} `json:"task"`
    	}
    	if err := json.Unmarshal(res.Result, &added); err != nil {
    		return fmt.Errorf("parse the created task: %w", err)
    	}
    	defer del(ctx, core, "tasks.task.delete", b24.Params{"taskId": added.Task.ID})
    	fmt.Printf("task %d created with the binding %s\n", added.Task.ID, binding)

    	// --- check: the binding was actually retained
    	// Without UF_CRM_TASK in select, the binding will not be in the response: it is a system field,
    	// by default it is not returned.
    	res, err = core.Call(ctx, "tasks.task.get", b24.Params{
    		"taskId": added.Task.ID,
    		"select": []string{"ID", "UF_CRM_TASK"},
    	}, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("tasks.task.get: %w", err)
    	}

    	// UF_CRM_TASK was requested, but in the response the field is called ufCrmTask. UnwrapFold
    	// compares names ignoring case and underscores, so such a
    	// renaming does not break it.
    	raw, ok := b24.UnwrapFold(res.Result, "task", "UF_CRM_TASK")
    	if !ok || b24.IsEmpty(raw) {
    		return fmt.Errorf("the binding was not retained on task %d", added.Task.ID)
    	}
    	var bindings []string
    	if err := json.Unmarshal(raw, &bindings); err != nil {
    		return fmt.Errorf("parse bindings: %w", err)
    	}
    	fmt.Printf("in task %d the \"CRM items\" field = %v\n", added.Task.ID, bindings)
    	return nil
    }

    // --- helpers: data setup and cleanup

    // addType creates a smart process and returns its entityTypeId and id.
    func addType(ctx context.Context, core *b24.Core, title string) (int, b24.ID, error) {
    	// Neither setting is enough on its own: isUseInUserfieldEnabled allows
    	// a smart process in custom fields at all, while linkedUserFields puts
    	// it specifically in the UF_CRM_TASK task field. Without the second one, the binding is not retained.
    	//
    	// isRecyclebinEnabled is disabled deliberately: an item in the recycle bin still
    	// counts as an item, and crm.type.delete refuses to delete a type
    	// that has items.
    	res, err := core.Call(ctx, "crm.type.add", b24.Params{
    		"fields": b24.Params{
    			"title":                   title,
    			"isUseInUserfieldEnabled": "Y",
    			"linkedUserFields":        b24.Params{"TASKS_TASK|UF_CRM_TASK": "true"},
    			"isRecyclebinEnabled":     "N",
    		},
    	})
    	if err != nil {
    		// The error code is compared with errors.Is rather than as a string: a typo in the
    		// literal would compile and silently take a different branch.
    		if errors.Is(err, b24.Code("CREATE_DYNAMIC_TYPE_RESTRICTED")) {
    			return 0, 0, fmt.Errorf("a smart process cannot be created on this portal: %w", err)
    		}
    		return 0, 0, fmt.Errorf("crm.type.add: %w", err)
    	}
    	var out struct {
    		Type struct {
    			ID           b24.ID `json:"id"`
    			EntityTypeID int    `json:"entityTypeId"`
    		} `json:"type"`
    	}
    	if err := json.Unmarshal(res.Result, &out); err != nil {
    		return 0, 0, fmt.Errorf("parse smart process: %w", err)
    	}
    	return out.Type.EntityTypeID, out.Type.ID, nil
    }

    func addItem(ctx context.Context, core *b24.Core, entityTypeID int, title string) (b24.ID, error) {
    	res, err := core.Call(ctx, "crm.item.add", b24.Params{
    		"entityTypeId": entityTypeID,
    		"fields":       b24.Params{"title": title},
    	})
    	if err != nil {
    		return 0, fmt.Errorf("crm.item.add: %w", err)
    	}
    	raw, ok := b24.Unwrap(res.Result, "item", "id")
    	if !ok {
    		return 0, fmt.Errorf("no item.id in %s", res.Result)
    	}
    	var id b24.ID
    	return id, json.Unmarshal(raw, &id)
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

## Verify the Result

Open the created task in Bitrix24. The linked SPA item is displayed in the task card in the "CRM items" field.

Via REST, the binding is verified using the [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) method with the following parameters:

- `taskId` — `3731`, the ID of the task created in the previous method's result

- `select` — `UF_CRM_TASK`, the "CRM items" field. Without this field in `select`, the method will not return the binding: `UF_CRM_TASK` refers to system fields that are not returned by default. In the request, the field name is written in uppercase, while in the response, it is returned in camelCase — `ufCrmTask`

{% list tabs %}

- JS
  
    ```javascript
    const checkResponse = await $b24.actions.v2.call.make({
        method: 'tasks.task.get',
        params: {
            taskId: 3731, // Task ID
            select: ['ID', 'UF_CRM_TASK'] // selectable fields
        },
        requestId: 'task-get'
    })

    if (!checkResponse.isSuccess) {
        throw new Error(checkResponse.getErrorMessages().join('; '))
    }

    const checkedTask = checkResponse.getData().result.task
    ```

- PHP
  
    ```php
    $result = $serviceBuilder->core->call(
        'tasks.task.get',
        [
            'taskId' => 3731, // Task ID
            'select' => ['ID', 'UF_CRM_TASK'] // selectable fields
        ]
    )->getResponseData()->getResult();
    ```

- Python

    ```python
    result = client.tasks.task.get(
        bitrix_id=3731,
        select=["ID", "UF_CRM_TASK"],
    ).response.result
    ```

- Go

    ```go
    // Without UF_CRM_TASK in select, the binding will not be in the response: it is a system field,
    // by default it is not returned.
    res, err = core.Call(ctx, "tasks.task.get", b24.Params{
    	"taskId": added.Task.ID,
    	"select": []string{"ID", "UF_CRM_TASK"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.task.get: %w", err)
    }

    // UF_CRM_TASK was requested, but in the response the field is called ufCrmTask. UnwrapFold
    // compares names ignoring case and underscores, so such a
    // renaming does not break it.
    raw, ok := b24.UnwrapFold(res.Result, "task", "UF_CRM_TASK")
    if !ok || b24.IsEmpty(raw) {
    	return fmt.Errorf("the binding was not retained on task %d", added.Task.ID)
    }
    var bindings []string
    if err := json.Unmarshal(raw, &bindings); err != nil {
    	return fmt.Errorf("parse bindings: %w", err)
    }
    ```

{% endlist %}

The scenario is successful if the `ufCrmTask` field in the response contains the collected value `Tb1_29`.

Abbreviated response:

```json
{
    "result": {
        "task": {
            "id": "3731",
            "ufCrmTask": ["Tb1_29"],
            "ufTaskWebdavFiles": false,
            "ufMailMessage": null,
            "favorite": "N",
            "group": [],
            ...
        }
    }
}
```

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Code** | **Reason and action** ||
|| `NOT_FOUND` | SPA not found. In `entityTypeId` of the [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) method, an identifier for a non-existent SPA was passed — take `ID` from the [crm.enum.ownertype](../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) response ||
|| `ENTITY_TYPE_NOT_SUPPORTED` | A value that does not belong to smart processes was passed in `entityTypeId`. Take `ID` from the [crm.enum.ownertype](../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) response instead of using an arbitrary number ||
|| `ERROR_CORE` | A mandatory field value was not entered. Mandatory custom task fields are configured on the portal — get their composition using the [tasks.task.getFields](../../api-reference/tasks/tasks-task-get-fields.md) method and pass them to `fields` ||
|| `INVALID_ARG_VALUE` | The field is unavailable for filtering or an incorrect value was passed to it. Check `filter` in [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) ||
|| `allowed_only_intranet_user` | The action in [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) is allowed only for intranet users. Check which user the incoming webhook was created for ||
|| `ERROR_CORE` | Task name or assignee is not specified. Fill in `TITLE` and `RESPONSIBLE_ID` ||
|| `ERROR_CORE` | The user specified in the "Assignee" field was not found. An identifier for a non-existent user was passed in `RESPONSIBLE_ID` ||
|| `100` | Mandatory parameters were not passed. Check `fields` in [tasks.task.add](../../api-reference/tasks/tasks-task-add.md), as well as `taskId` and `select` in [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) ||
|| `0` | An incorrect type value was specified in the `taskId` parameter of the [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) method ||
|#

A task may be created without an error but with an empty `ufCrmTask` field. In this case, check the binding value and the SPA configurations:

- The SPA is added to `linkedUserFields` via the `TASKS_TASK|UF_CRM_TASK` key, and not only marked with the `isUseInUserfieldEnabled` option. This is the most common reason: until both settings are specified, the `UF_CRM_TASK` field does not accept items from this SPA.
- The value was collected using the formula from step 3 — `Tb1_29`, rather than `Tb1 29`, `Tb1-29`, or `177_29` using a type identifier instead of an item identifier.

If [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) returns an empty `result`, the task with that identifier does not exist or the webhook user does not have access to it. This is not an indication that the binding failed to save.

Repeat the scenario from the step that returned an error. Steps 1 and 2 do not create anything and can be executed any number of times. If [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) returns an error, the task was not created: correct the fields and repeat only step 3. If the task was created but the binding is incorrect, do not create a new task — update the `UF_CRM_TASK` field using the [tasks.task.update](../../api-reference/tasks/tasks-task-update.md) method.

## Key Considerations

- The SPA symbolic code is calculated from `entityTypeId`: the number is converted to hexadecimal and receives the prefix `T`. For example, `entityTypeId`: `177` results in `b1` and code `Tb1`.
- `UF_CRM_TASK` accepts an array, so you can bind multiple CRM objects of different types to a single task, for example `["Tb1_29", "D_10"]`.
- You can bind more than just SPAs to a task. By default, the `UF_CRM_TASK` field accepts a lead, contact, company, deal, and order — insert the symbolic code of the required type: `L`, `C`, `CO`, `D`, `O`. For these types, the step with [crm.enum.ownertype](../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) is not required: the codes are constant and listed in the [CRM Object Types Table](../../api-reference/crm/data-types.md#object_type). Estimates and invoices are not included in this default list.
- [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) returns 50 items per page. If the required item is not found, refine the filter or iterate through pages using the `start` parameter.
- Rerunning the example creates a new task.

## Continue Learning

- [Create a Task tasks.task.add](../../api-reference/tasks/tasks-task-add.md)
- [Get a Task by Identifier tasks.task.get](../../api-reference/tasks/tasks-task-get.md)
- [Update a Task tasks.task.update](../../api-reference/tasks/tasks-task-update.md)
- [Get a List of CRM Object Types crm.enum.ownertype](../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md)
- [Get a List of SPA Items crm.item.list](../../api-reference/crm/universal/crm-item-list.md)
- [Value Format for Binding to CRM Items](../../api-reference/crm/data-types.md#crm-binding-format)
- [{#T}](./how-to-create-task-with-file.md)
