# How to Attach a Task to a Smart Process

> Scope: [`crm, tasks`](../../api-reference/scopes/permissions.md)
> 
> Who can execute the method: users with access to the CRM and tasks sections

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../ai-tools/mcp.md) so that the assistant uses the official REST documentation.

{% endnote %}

The key parameter for attaching a task to a CRM object is the [object type identifier](../../api-reference/crm/data-types.md#object_type). This identifier indicates which type of object the relationship will be added to: a deal, a lead, or a specific smart process.

To create a task and attach it to an SPA, we will sequentially execute three methods:

1. [crm.enum.ownertype](../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) — retrieve the `entityTypeId` and `SYMBOL_CODE_SHORT` of the SPA

2. [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) — retrieve the SPA item with the `entityTypeId` parameter

3. [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) — create a task and link it to the SPA item using `SYMBOL_CODE_SHORT`
   
## 1. Retrieve SPA Identifiers {#SPA-ids}

To retrieve the SPA identifier, use the [crm.enum.ownertype](../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method. The method is called without parameters and returns an enumeration of all CRM object types.

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/')

    const response = await $b24.actions.v2.call.make({
        method: 'crm.enum.ownertype',
        params: {},
        requestId: 'crm-enum-ownertype'
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    const result = response.getData().result
    ```

- PHP

    ```php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $result = $serviceBuilder->getCRMScope()->enum()->ownerType()->getItems();
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    result = client.crm.enum.ownertype().response.result
    ```

{% endlist %}

The method returns four different identifiers:

```JSON
     "ID": 130, // entityTypeId — obtained to find a CRM item by filter
     "NAME": "All inclusive", // name
     "SYMBOL_CODE": "DYNAMIC_130", // character code
     "SYMBOL_CODE_SHORT": "T82" // short character code — obtained to link a CRM item to a task
```

`ID` is retrieved to find a CRM item by filter.

`SYMBOL_CODE_SHORT` is retrieved to bind a CRM item to a task.

```JSON
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
            "NAME": "Details",
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
    ],
}
```

As a result, we have obtained a list of all CRM object types in Bitrix24 with their identifiers. For the following requests, we will use `ID`: `177` and `SYMBOL_CODE_SHORT`: `Tb1` for the "Equipment Procurement" SPA.

## 2. Retrieving the Smart Process Element ID {#element-id}

To retrieve the SPA item ID, use the [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) method with the following parameters:

-  `entityTypeId` — `177`, where the value equals `ID` from the previous method's result

-  `filter[title]` — specify the item name for the search  

{% list tabs %}

- JS
  
    ```javascript
    const response = await $b24.actions.v2.call.make({
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

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    const result = response.getData().result
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

{% endlist %}

As a result, we have obtained the SPA item ID — the parameter required for the next request.

```JSON
{
    "result": {
        "items": [
            {
                "id": 29,
                "title": "Washing machine"
            }
        ]
    },
    "total": 1,
}
```

## 3. Create a Task Bound to an SPA Item

To create a task, use the [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) method with the following parameters:

-  `UF_CRM_TASK` — specify the value of `Tb1_29`. This is the short character code of the type `SYMBOL_CODE_SHORT`: `Tb1` from the [crm.enum.ownertype](./how-to-connect-task-to-spa.md#SPA-ids) results and the SPA item ID `id`: `29` from the [crm.item.list](./how-to-connect-task-to-spa.md#element-id) results

-  `TITLE` — the task name, a required field. The task will not be created without a name

-  `CREATED_BY` — the ID of the task creator; this field cannot be empty. If it is not filled, the user sending the request will automatically become the creator

-  `RESPONSIBLE_ID` — the ID of the task assignee, a required field. The task will not be created without an assignee

{% list tabs %}

- JS
  
    ```javascript
    const response = await $b24.actions.v2.call.make({
        method: 'tasks.task.add',
        params: {
            fields: {
                TITLE: 'task for test', // task name
                RESPONSIBLE_ID: 1, // performer
                UF_CRM_TASK: [ // array of CRM items
                    'Tb1_29'
                ]
            }
        },
        requestId: 'task-add'
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    const result = response.getData().result
    ```

- PHP
  
    ```php
    $result = $serviceBuilder->core->call(
        'tasks.task.add',
        [
            'fields' => [
                'TITLE' => 'task for test', // task name
                'RESPONSIBLE_ID' => 1, // performer
                'UF_CRM_TASK' => [ // array of CRM items
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

{% endlist %}

As a result, a task was created with ID `3731`.

```JSON
{
    "result": {
        "task": {
            "id": "3731",
            "parentId": null,
            "title": "task for test",
            "description": "",
            "mark": null,
            "priority": "1",
            "multitask": "N",
            "notViewed": "N",
            "replicate": "N",
            "stageId": "0",
            "createdBy": "1",
            "createdDate": "2025-01-20T14:30:58+02:00",
            "responsibleId": "1",
            "changedBy": "1",
            "changedDate": "2025-01-20T14:30:58+02:00",
            "statusChangedBy": null,
            "closedBy": null,
            "closedDate": null,
            "activityDate": "2025-01-20T14:30:58+02:00",
            "dateStart": null,
            "deadline": null,
            "startDatePlan": null,
            "endDatePlan": null,
            "guid": "{34429425-80c6-4927-83bd-220e67bcc202}",
            "xmlId": null,
            "commentsCount": null,
            "serviceCommentsCount": null,
            "allowChangeDeadline": "N",
            "allowTimeTracking": "N",
            "taskControl": "N",
            "addInReport": "N",
            "forkedByTemplateId": null,
            "timeEstimate": "0",
            "timeSpentInLogs": null,
            "matchWorkTime": "N",
            "forumTopicId": null,
            "forumId": null,
            "siteId": "s1",
            "subordinate": "Y",
            "exchangeModified": null,
            "exchangeId": null,
            "outlookVersion": "1",
            "viewedDate": null,
            "sorting": null,
            "durationFact": null,
            "isMuted": "N",
            "isPinned": "N",
            "isPinnedInGroup": "N",
            "flowId": null,
            "descriptionInBbcode": "Y",
            "status": "2",
            "statusChangedDate": "2025-01-20T14:30:58+02:00",
            "durationPlan": null,
            "durationType": "days",
            "favorite": "N",
            "groupId": "0",
            "auditors": [],
            "accomplices": [],
            "checklist": [],
            "group": [],
            "creator": {
                "id": "1",
                "name": "Viola",
                "link": "/company/personal/user/1/",
                "icon": "https://your-domain.bitrix24.com/b13743910/resize_cache/2267/c0120a8d7c10d63c83e32398d1ec4d9e/main/c7b/c7bd44b1babaa5448125dd97d038ce1b/d5fb56b94dc2c3cd8c006a2c595a4895.jpg",
                "workPosition": ""
            },
            "responsible": {
                "id": "1",
                "name": "Viola",
                "link": "/company/personal/user/1/",
                "icon": "https://your-domain.bitrix24.com/b13743910/resize_cache/2267/c0120a8d7c10d63c83e32398d1ec4d9e/main/c7b/c7bd44b1babaa5448125dd97d038ce1b/d5fb56b94dc2c3cd8c006a2c595a4895.jpg",
                "workPosition": ""
            },
            "accomplicesData": [],
            "auditorsData": [],
            "newCommentsCount": 0,
            "action": {
                "accept": false,
                "decline": false,
                "complete": true,
                "approve": false,
                "disapprove": false,
                "start": true,
                "pause": false,
                "delegate": true,
                "remove": true,
                "edit": true,
                "defer": true,
                "renew": false,
                "create": true,
                "changeDeadline": true,
                "checklistAddItems": true,
                "addFavorite": true,
                "deleteFavorite": false,
                "rate": true,
                "take": false,
                "edit.originator": false,
                "checklist.reorder": true,
                "elapsedtime.add": true,
                "dayplan.timer.toggle": false,
                "edit.plan": true,
                "checklist.add": true,
                "favorite.add": true,
                "favorite.delete": false
            },
            "checkListTree": {
                "nodeId": 0,
                "fields": {
                    "id": null,
                    "copiedId": null,
                    "entityId": null,
                    "userId": 1,
                    "createdBy": null,
                    "parentId": null,
                    "title": "",
                    "sortIndex": null,
                    "displaySortIndex": "",
                    "isComplete": false,
                    "isImportant": false,
                    "completedCount": 0,
                    "members": [],
                    "attachments": []
                },
                "action": [],
                "descendants": []
            },
            "checkListCanAdd": true
        }
    },
}
```

## Verifying the Created Task

The received result does not contain information about linked CRM items. To verify that the SPA item was successfully attached to the task, call the [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) method with the following parameters:

- `taskId` — `3731`, the ID of the created task from the previous method's result

- `select` — `UF_CRM_TASK`, field "CRM Link". The method [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) will not return the link field without `UF_CRM_TASK` in `select`
  
{% list tabs %}

- JS
  
    ```javascript
    const response = await $b24.actions.v2.call.make({
        method: 'tasks.task.get',
        params: {
            taskId: 3731, // task ID
            select: ['ID', 'UF_CRM_TASK'] // selectable fields
        },
        requestId: 'task-get'
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    const result = response.getData().result
    ```

- PHP
  
    ```php
    $result = $serviceBuilder->core->call(
        'tasks.task.get',
        [
            'taskId' => 3731, // task ID
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

{% endlist %}

As a result, we obtained the value of the `ufCrmTask` field: `Tb1_29`. The SPA item was successfully attached.

```JSON
{
    "result": {
        "task": {
            "id": "3731",
            "ufCrmTask": ["Tb1_29"],
            "ufTaskWebdavFiles": false,
            "ufMailMessage": null,
            "ufAuto615763798639": null,
            "ufAuto885808697713": null,
            "ufAuto168639979930": null,
            "ufAuto441714695872": null,
            "ufAuto179124361273": null,
            "favorite": "N",
            "group": [],
            "action": {
                "accept": false,
                "decline": false,
                "complete": true,
                "approve": false,
                "disapprove": false,
                "start": true,
                "pause": false,
                "delegate": true,
                "remove": true,
                "edit": true,
                "defer": true,
                "renew": false,
                "create": true,
                "changeDeadline": true,
                "checklistAddItems": true,
                "addFavorite": true,
                "deleteFavorite": false,
                "rate": true,
                "take": false,
                "edit.originator": false,
                "checklist.reorder": true,
                "elapsedtime.add": true,
                "dayplan.timer.toggle": false,
                "edit.plan": true,
                "checklist.add": true,
                "favorite.add": true,
                "favorite.delete": false
            }
        }
    },
}
```

## Code Example

{% list tabs %}

- JS
    
    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/')

    // Variables for user data input
    const smartProcessName = 'smart_process_name'; // Smart process name
    const itemName = 'element_name'; // Smart process element name
    const responsibleId = 'responsible_ID'; // task responsible ID
    const taskTitle = 'task_name'; // Task name

    // Function to create a task linked to a smart process element
    async function createTaskWithSmartProcess() {
        // Getting entity type and smart process IDs
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

        // Task creation
        const taskResponse = await $b24.actions.v2.call.make({
            method: 'tasks.task.add',
            params: {
                fields: {
                    TITLE: taskTitle, // Using the entered task name
                    RESPONSIBLE_ID: responsibleId, // Adding the responsible ID
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

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    // Variables for user data input
    $smartProcessName = 'smart_process_name'; // Smart process name
    $itemName = 'element_name'; // Smart process element name
    $responsibleId = 'responsible_ID'; // task responsible ID
    $taskTitle = 'task_name'; // Task name

    // Function to create a task linked to a smart process element
    function createTaskWithSmartProcess($serviceBuilder, $smartProcessName, $itemName, $responsibleId, $taskTitle) {
        // Getting entity type and smart process IDs
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

        // Task creation
        try {
            $taskResult = $serviceBuilder->core->call('tasks.task.add', [
                'fields' => [
                    'TITLE' => $taskTitle, // Using the entered task name
                    'RESPONSIBLE_ID' => $responsibleId, // Adding the responsible ID
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
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    smart_process_name = "smart_process_name"
    item_name = "element_name"
    responsible_id = "responsible_ID"
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
            webhook_token="user_id/webhook_key",
        )
    )

    create_task_with_smart_process(client, smart_process_name, item_name, responsible_id, task_title)
    ```

{% endlist %}
