# How to Attach a Task to a Smart Process

> Scope: [`crm, tasks`](../../api-reference/scopes/permissions.md)
> 
> Who can execute the method: users with access to the CRM and tasks sections

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../sdk/mcp.md) so that the assistant uses the official REST documentation.

{% endnote %}

The key parameter for attaching a task to a CRM entity is the [object type identifier](../../api-reference/crm/data-types.md#object_type). This identifier indicates which type of object the relationship will be added to: a deal, a lead, or a specific smart process.

To create a task and attach it to a smart process, we will sequentially execute three methods:

1. [crm.enum.ownertype](../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) — retrieve the `entityTypeId` and `SYMBOL_CODE_SHORT` of the smart process.

2. [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) — retrieve the smart process entity using the `entityTypeId` parameter.

3. [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) — create a task and link it to the smart process entity using `SYMBOL_CODE_SHORT`.

## 1. Retrieving Smart Process Identifiers {#SPA-ids}

To obtain the smart process identifier, we use the [crm.enum.ownertype](../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method. This method is called without parameters and returns an enumeration of all CRM object types.

{% include [Examples Note](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```JavaScript
    BX24.callMethod(
    "crm.enum.ownertype",
    {} 
    );     
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.enum.ownertype',
        []
    );
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            auth_token="your-webhook-token",
        )
    )

    result = client.crm.enum.ownertype().response.result
    ```

{% endlist %}

The method returns four different identifiers:

```JSON
     "ID": 130, // entityTypeId — obtained to find the CRM entity by filter
     "NAME": "All Inclusive", // name
     "SYMBOL_CODE": "DYNAMIC_130", // symbolic code
     "SYMBOL_CODE_SHORT": "T82" // short symbolic code — obtained to link the CRM entity to the task
```

`ID` is obtained to find the CRM entity by filter.

`SYMBOL_CODE_SHORT` is obtained to link the CRM entity to the task.

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
            "NAME": "Estimate",
            "SYMBOL_CODE": "QUOTE",
            "SYMBOL_CODE_SHORT": "Q"
        },
        {
            "ID": 8,
            "NAME": "Requisite",
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
            "NAME": "Company Document",
            "SYMBOL_CODE": "SMART_B2E_DOC",
            "SYMBOL_CODE_SHORT": "SBD"
        },
        {
            "ID": 177,
            "NAME": "Equipment Purchase",
            "SYMBOL_CODE": "DYNAMIC_177",
            "SYMBOL_CODE_SHORT": "Tb1"
        },
        {
            "ID": 156,
            "NAME": "Purchase",
            "SYMBOL_CODE": "DYNAMIC_156",
            "SYMBOL_CODE_SHORT": "T9c"
        }
    ]
}
```

As a result, we obtained a list of all CRM object types in Bitrix24 with their identifiers. For the next requests, we will use `ID`: `177` and `SYMBOL_CODE_SHORT`: `Tb1` for the smart process "Equipment Purchase."

## 2. Retrieving the Smart Process Element ID {#element-id}

To obtain the ID of the smart process element, we use the [crm.item.list](../../api-reference/crm/universal/crm-item-list.md) method with the following parameters:

-  `entityTypeId` — `177`, which is the value of `ID` from the previous method's result.

-  `filter[title]` — specify the name of the element to search for.  

{% list tabs %}

- JS
  
    ```javascript
       BX24.callMethod(
        'crm.item.list',
        {
            entityTypeId: 177, // ID from the result of crm.enum.ownertype
            select: [
                "id", // selected fields
                "title",
            ],
            filter: {
                "title": "Washing Machine", // name of the element
            },
        }
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.list',
        [
            'entityTypeId' => 177, // ID from the result of crm.enum.ownertype
            'select' => [
                'id', // selected fields
                'title',
            ],
            'filter' => [
                'title' => 'Washing Machine', // name of the element
            ],
        ]
    );
    ```

- Python
  
    ```python
    result = client.crm.item.list(
        entity_type_id=177,
        select=["id", "title"],
        filter={
            "title": "Washing Machine",
        },
    ).response.result
    ```

{% endlist %}

As a result, we obtained the ID of the smart process element — a parameter necessary for the next request.

```JSON
{
    "result": {
        "items": [
            {
                "id": 29,
                "title": "Washing Machine"
            }
        ]
    },
    "total": 1
}
```

## 3. Creating a Task Linked to the Smart Process Element

To create a task, we use the [tasks.task.add](../../api-reference/tasks/tasks-task-add.md) method with the following parameters:

-  `UF_CRM_TASK` — specify the value `Tb1_29`. This is the short symbolic code of type `SYMBOL_CODE_SHORT`: `Tb1` from the results of [crm.enum.ownertype](./how-to-connect-task-to-spa.md#SPA-ids) and the ID of the smart process element `id`: `29` from the results of [crm.item.list](./how-to-connect-task-to-spa.md#element-id).

-  `TITLE` — the title of the task, a required field. Without a title, the task will not be created.

-  `CREATED_BY` — the ID of the task Creator, this field cannot be empty. If not filled, the Creator will automatically be the one sending the request.

-  `RESPONSIBLE_ID` — the ID of the task Participant, a required field. Without a Participant, the task will not be created.
  
{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        'tasks.task.add',
        {
            fields: {
                TITLE: 'task for test', // task title
                RESPONSIBLE_ID: 1, // participant
                UF_CRM_TASK: [ // array of CRM entities
                    "Tb1_29"
                ]
            }
        }
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.add',
        [
            'fields' => [
                'TITLE' => 'task for test', // task title
                'RESPONSIBLE_ID' => 1, // participant
                'UF_CRM_TASK' => [ // array of CRM entities
                    'Tb1_29'
                ]
            ]
        }
    );
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

As a result, we created a task with ID `3731`.

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
    }
}
```

## Verifying the Created Task

The obtained result does not contain information about the linked CRM entities. To check if the smart process element has been successfully attached to the task, we will execute the [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) method with the following parameters:

-  `taskId` — `3731`, the ID of the created task from the result of the previous method.

-  `select` — `UF_CRM_TASK`, the field "Link to CRM entities." The [tasks.task.get](../../api-reference/tasks/tasks-task-get.md) method will not return the link field without `UF_CRM_TASK` in `select`.
  
{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        'tasks.task.get',
        {
            taskId: 3731, // task ID
            select: ['ID', 'UF_CRM_TASK'] // selected fields
        }
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.get',
        [
            'taskId' => 3731, // task ID
            'select' => ['ID', 'UF_CRM_TASK'] // selected fields
        ]
    );
    ```

- Python
  
    ```python
    result = client.tasks.task.get(
        bitrix_id=3731,
        select=["ID", "UF_CRM_TASK"],
    ).response.result
    ```
  

{% endlist %}

As a result, we obtained the value of the `ufCrmTask` field: `Tb1_29`. The smart process element has been successfully attached.

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
    }
}
```

## Code Example

{% list tabs %}

- JS
    
    ```JavaScript
    // Variables for user input
    var smartProcessName = 'smart_process_name'; // Name of the smart process
    var itemName = 'item_name'; // Name of the smart process element
    var responsibleId = 'RESPONSIBLE_ID'; // ID of the task responsible person
    var taskTitle = 'task_title'; // Title of the task

    // Function to create a task linked to the smart process element
    function createTaskWithSmartProcess() {
        // Retrieving entity type and smart process identifiers
        BX24.callMethod(
            'crm.enum.ownertype',
            {},
            function(result) {
                if (result.error()) {
                    console.error('Error retrieving entity types:', result.error());
                    return;
                }

                // Finding the required smart process
                var smartProcess = result.data().find(function(process) {
                    return process.NAME === smartProcessName;
                });

                if (!smartProcess) {
                    console.error('Smart process not found');
                    return;
                }

                var symbolCodeShort = smartProcess.SYMBOL_CODE_SHORT;

                // Searching for the smart process element using a title filter
                BX24.callMethod(
                    'crm.item.list',
                    {
                        entityTypeId: smartProcess.ID,
                        select: ["id", "title"],
                        filter: { "title": itemName }
                    },
                    function(itemResult) {
                        if (itemResult.error()) {
                            console.error('Error retrieving smart process elements:', itemResult.error());
                            return;
                        }

                        if (itemResult.data().items.length === 0) {
                            console.error('Smart process element not found');
                            return;
                        }

                        var itemId = itemResult.data().items[0].id;

                        // Creating the task
                        BX24.callMethod(
                            'tasks.task.add',
                            {
                                fields: {
                                    TITLE: taskTitle, // Using the entered task title
                                    RESPONSIBLE_ID: responsibleId, // Adding the responsible ID
                                    UF_CRM_TASK: [symbolCodeShort + '_' + itemId]
                                }
                            },
                            function(taskResult) {
                                if (taskResult.error()) {
                                    console.error('Error creating task:', taskResult.error());
                                } else {
                                    console.log('Task successfully created!', taskResult.data());
                                }
                            }
                        );
                    }
                );
            }
        );
    }

    // Calling the function to create the task
    createTaskWithSmartProcess();
    ```

- PHP
  
    ```php
    require_once('crest.php');

    // Variables for user input
    $smartProcessName = 'smart_process_name'; // Name of the smart process
    $itemName = 'item_name'; // Name of the smart process element
    $responsibleId = 'RESPONSIBLE_ID'; // ID of the task responsible person
    $taskTitle = 'task_title'; // Title of the task

    // Function to create a task linked to the smart process element
    function createTaskWithSmartProcess($smartProcessName, $itemName, $responsibleId, $taskTitle) {
        // Retrieving entity type and smart process identifiers
        $result = CRest::call('crm.enum.ownertype', []);

        if (isset($result['error'])) {
            echo 'Error retrieving entity types: ' . $result['error_description'];
            return;
        }

        // Finding the required smart process
        $smartProcess = null;
        foreach ($result['result'] as $process) {
            if ($process['NAME'] === $smartProcessName) {
                $smartProcess = $process;
                break;
            }
        }

        if (!$smartProcess) {
            echo 'Smart process not found';
            return;
        }

        $symbolCodeShort = $smartProcess['SYMBOL_CODE_SHORT'];

        // Searching for the smart process element using a title filter
        $itemResult = CRest::call('crm.item.list', [
            'entityTypeId' => $smartProcess['ID'],
            'select' => ['id', 'title'],
            'filter' => ['title' => $itemName]
        ]);

        if (isset($itemResult['error'])) {
            echo 'Error retrieving smart process elements: ' . $itemResult['error_description'];
            return;
        }

        if (count($itemResult['result']['items']) === 0) {
            echo 'Smart process element not found';
            return;
        }

        $itemId = $itemResult['result']['items'][0]['id'];

        // Creating the task
        $taskResult = CRest::call('tasks.task.add', [
            'fields' => [
                'TITLE' => $taskTitle, // Using the entered task title
                'RESPONSIBLE_ID' => $responsibleId, // Adding the responsible ID
                'UF_CRM_TASK' => [$symbolCodeShort . '_' . $itemId]
            ]
        ]);

        if (isset($taskResult['error'])) {
            echo 'Error creating task: ' . $taskResult['error_description'];
        } else {
            echo 'Task successfully created!';
            print_r($taskResult['result']);
        }
    }

    // Calling the function to create the task
    createTaskWithSmartProcess($smartProcessName, $itemName, $responsibleId, $taskTitle);
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    smart_process_name = "smart_process_name"
    item_name = "item_name"
    responsible_id = "RESPONSIBLE_ID"
    task_title = "task_title"


    def create_task_with_smart_process(client, smart_process_name, item_name, responsible_id, task_title):
        try:
            result = client.crm.enum.ownertype().response.result
        except BitrixAPIError as error:
            print(f"Error retrieving entity types: {error}")
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
            print(f"Error retrieving smart process elements: {error}")
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
            print("Task successfully created!")
            print(task_result)


    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            auth_token="your-webhook-token",
        )
    )

    create_task_with_smart_process(client, smart_process_name, item_name, responsible_id, task_title)
    ```

{% endlist %}