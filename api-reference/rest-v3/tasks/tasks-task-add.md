# Add Task tasks.task.add

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.add` adds a new task.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Task fields. To create a task, fill in the [required fields](#fields); without them, the creation operation will not be executed.
||
|#

### Parameter fields {#fields}

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **title***
[`string`](../../data-types.md) | Task title ||
|| **creatorId***
[`integer`](../../data-types.md)  | Creator's identifier. 
The employee identifier can be obtained using the [user.get](../../user/user-get.md) method. ||
|| **responsibleId***
[`integer`](../../data-types.md)  | Executor's identifier.
The employee identifier can be obtained using the [user.get](../../user/user-get.md) method. ||
|#

{% note info "" %}

[Description of all task fields](./fields.md)

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The call to the new API differs by adding the parameter `/api/` in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.task.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"title":"Task Title","deadline":"2025-12-31T23:59:59+02:00","creatorId":29,"responsibleId":1,"crmItemIds":["L_1000959"]}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"title":"Task Title","deadline":"2025-12-31T23:59:59+02:00","creatorId":29,"responsibleId":1,"crmItemIds":["L_1000959"]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.task.add
    ```

- JS

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.add',
            {
                fields: {
                    title: 'Task Title',
                    deadline: '2025-12-31T23:59:59+02:00',
                    creatorId: 29,
                    responsibleId: 1,
                    crmItemIds: ['L_1000959']
                }
            }
        );
        
        const result = response.getData().result;
        console.info('Task created with ID', result.item?.id);
    }
    catch( error )
    {
        console.error(error);
    }
    ```

- PHP

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.add',
                [
                    'fields' => [
                        'title' => 'Task Title',
                        'deadline' => '2025-12-31T23:59:59+02:00',
                        'creatorId' => 29,
                        'responsibleId' => 1,
                        'crmItemIds' => ['L_1000959'],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating task: ' . $e->getMessage();
    }
    ```

- BX24.js

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```js
    BX24.callMethod(
        'tasks.task.add',
        {
            fields: {
                title: 'Task Title',
                deadline: '2025-12-31T23:59:59+02:00',
                creatorId: 29,
                responsibleId: 1,
                crmItemIds: ['L_1000959']
            }
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.add',
        [
            'fields' => [
                'title' => 'Task Title',
                'deadline' => '2025-12-31T23:59:59+02:00',
                'creatorId' => 29,
                'responsibleId' => 1,
                'crmItemIds' => ['L_1000959']
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "item": {
            "id": 3839,
            "title": "Task Title",
            "description": "",
            "deadline": "2026-01-01T00:59:59+03:00",
            "needsControl": false,
            "startPlan": null,
            "endPlan": null,
            "fileIds": null,
            "checklist": [],
            "epicId": null,
            "storyPoints": null,
            "priority": "average",
            "status": "pending",
            "statusChanged": null,
            "parentId": null,
            "containsChecklist": false,
            "containsSubTasks": false,
            "containsRelatedTasks": false,
            "containsGanttLinks": false,
            "containsPlacements": true,
            "containsResults": false,
            "numberOfReminders": 0,
            "chatId": 2603,
            "plannedDuration": 0,
            "actualDuration": 0,
            "durationType": "days",
            "started": null,
            "estimatedTime": 0,
            "replicate": false,
            "changed": "2025-12-11T12:25:33+03:00",
            "closed": null,
            "activity": "2025-12-11T12:25:33+03:00",
            "guid": "{13d2c44c-730e-45dd-b99c-fdaad4e3c1fa}",
            "xmlId": null,
            "exchangeId": null,
            "exchangeModified": null,
            "outlookVersion": 1,
            "mark": "none",
            "allowsChangeDeadline": false,
            "allowsTimeTracking": false,
            "matchesWorkTime": false,
            "addInReport": null,
            "isMultitask": false,
            "siteId": "s1",
            "deadlineCount": null,
            "declineReason": null,
            "forumTopicId": null,
            "link": "\/company\/personal\/user\/1\/tasks\/task\/view\/3839\/",
            "rights": {
                "read": true,
                "watch": true,
                "mute": true,
                "createResult": true,
                "edit": true,
                "remove": true,
                "complete": true,
                "approve": false,
                "disapprove": false,
                "start": true,
                "take": false,
                "delegate": true,
                "defer": true,
                "renew": false,
                "deadline": true,
                "datePlan": true,
                "changeDirector": false,
                "changeResponsible": true,
                "changeAccomplices": true,
                "pause": false,
                "timeTracking": false,
                "mark": true,
                "changeStatus": true,
                "reminder": true,
                "addAuditors": true,
                "elapsedTime": true,
                "favorite": true,
                "checklistAdd": true,
                "checklistEdit": true,
                "checklistSave": true,
                "checklistToggle": true,
                "automate": true,
                "resultEdit": false,
                "completeResult": true,
                "removeResult": false,
                "resultRead": false,
                "admin": true,
                "createSubtask": true,
                "copy": true,
                "saveAsTemplate": true,
                "attachFile": true,
                "detachFile": true,
                "detachParent": true,
                "createGanttDependence": true,
                "sort": false
            },
            "archiveLink": "\/bitrix\/tools\/disk\/uf.php?entityId=3839\u0026entity=TASKS_TASK\u0026fieldName=UF_TASK_WEBDAV_FILES\u0026signature=89f4f46e33905bcb0899c8cb9613a62cf8e104b182824e799d2aeb9d1e5bf526\u0026action=downloadArchiveByEntity\u0026ncc=1",
            "crmItemIds": [
                "L_1000959"
            ],
            "requireResult": false,
            "matchesSubTasksTime": false,
            "autocompleteSubTasks": false,
            "allowsChangeDatePlan": false,
            "maxDeadlineChangeDate": null,
            "maxDeadlineChanges": null,
            "requireDeadlineChangeReason": false,
            "inFavorite": [],
            "inPin": [],
            "inGroupPin": [],
            "inMute": [],
            "dependsOn": [],
            "scenarios": [
                "default"
            ]
        }
    },
    "time": {
        "start": 1765445133,
        "finish": 1765445134.139558,
        "duration": 1.1395580768585205,
        "processing": 1,
        "date_start": "2025-12-11T12:25:33+03:00",
        "date_finish": "2025-12-11T12:25:34+03:00",
        "operating_reset_at": 1765445733,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with response data ||
|| **item**
[`object`](../../data-types.md) | Object with task field values. [Description of task fields with related objects](./fields.md) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#


## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating the request object",
        "validation": [
            {
                "message": "The `deadline` field requires a `DateTime` data type for this request",
                "field": "deadline"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors  

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `title`
`responsibleId`
`creatorId`
`fields` | Required field `#FIELD#` is missing | Add the specified field to the request body ||
|| `#FIELD#` | The `#FIELD#` field requires a data type of `#TYPE#` for this request | Ensure that the provided value is of the correct type ||
|| `responsibleId` | The user specified in the "Executor" field was not found | Specify the identifier of an existing user in the `responsibleId` field ||
|| `creatorId` | "" | Specify the identifier of an existing user in the `creatorId` field ||
|| `parentId` | The task specified in the "Parent Task" field was not found | Specify the identifier of an existing task in the `parentId` field ||
|| `endPlan` | The end date in the scheduling is earlier than the start date | Specify an `endPlan` date later than `startPlan` ||
|| `endPlan` | The scheduling indicates a task duration that is too long | Reduce the date in the `endPlan` field ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-task-update.md)
- [{#T}](./tasks-task-chat-message-send.md)
- [{#T}](./tasks-task-delete.md)
- [{#T}](./tasks-task-file-attach.md)