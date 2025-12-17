# Get Task tasks.task.get

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.get` returns information about a task by its identifier.

Access to the data depends on permissions:
- administrators see all tasks,
- managers see their employees' tasks,
- others see only the tasks available to them.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by the old method of [getting the list of tasks](../../tasks/tasks-task-list.md) ||
|| **select** 
[`array`](../../data-types.md) | An array of fields that the method will return. If `select` is not specified, a basic set of task fields without related objects is returned.

For related objects, use a nested path with a dot, for example `["responsible.name","responsible.email"]` - the response will include the `responsible` object with the requested fields. [List of fields with related objects](./fields.md)
||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% note info "" %}

The call to the new API differs by adding the `/api/` parameter in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.task.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8017,"select":["responsible.name","responsible.email"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8017,"select":["responsible.name","responsible.email"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.task.get
    ```

- JS

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl, fetch. 

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.get',
            {
                id: 8017,
                select: [
                    'responsible.name',
                    'responsible.email'
                ]
            }
        );
        
        const result = response.getData().result;
        console.log('Fetched task:', result);
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl, fetch. 

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.get',
                [
                    'id' => 8017,
                    'select' => [
                        'responsible.name',
                        'responsible.email'
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching task: ' . $e->getMessage();
    }
    ```

- BX24.js

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl, fetch. 

    ```js
    BX24.callMethod(
        'tasks.task.get',
        {
            id: 8017,
            select: [
                'responsible.name',
                'responsible.email'
            ]
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl, fetch. 

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.get',
        [
            'id' => 8017,
            'select' => [
                'responsible.name',
                'responsible.email'
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
            "id": 3835,
            "title": "title",
            "description": "description",
            "responsible": {
                "name": "Alex",
                "email": "mail@bitrix.com"
            },
            "deadline": "2025-12-25T23:00:00+01:00",
            "needsControl": false,
            "startPlan": null,
            "endPlan": null,
            "fileIds": null,
            "checklist": [
                153,
                165
            ],
            "epicId": null,
            "storyPoints": null,
            "priority": "average",
            "status": "pending",
            "statusChanged": "2025-11-24T06:00:00+01:00",
            "parentId": null,
            "containsChecklist": true,
            "containsSubTasks": false,
            "containsRelatedTasks": false,
            "containsGanttLinks": false,
            "containsPlacements": true,
            "containsResults": false,
            "numberOfReminders": 0,
            "chatId": 2537,
            "plannedDuration": 0,
            "actualDuration": 0,
            "durationType": "days",
            "started": null,
            "estimatedTime": 0,
            "replicate": false,
            "changed": "2025-12-10T16:04:54+01:00",
            "closed": null,
            "activity": "2025-12-10T16:04:42+01:00",
            "guid": "{99502976-b2a2-4246-8d35-c6943b5ff242}",
            "xmlId": null,
            "exchangeId": null,
            "exchangeModified": null,
            "outlookVersion": 12,
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
            "link": "/company/personal/user/1/tasks/task/view/3835/",
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
            "archiveLink": "/bitrix/tools/disk/uf.php?entityId=3835&entity=TASKS_TASK&fieldName=UF_TASK_WEBDAV_FILES&signature=516ff0b54563ff935bbdf891590bed09bf4216db4fdf2df4df20425b7dd341e1&action=downloadArchiveByEntity&ncc=1",
            "crmItemIds": [
                "D_6529"
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
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Record with ID = `2` not found"
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors  

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `id` | Required field `id` is not specified | Add `id` to the request body ||
|| `id` | Field `id` requires data type `int` for this request | Ensure that the value is a number, not a string ||
|#

#### Data Not Found Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `id` | Record with ID = `2` not found | Specify `id` of an existing task ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Unknown field `status` for entity `TaskDto` | Specify existing fields in `select` for retrieving fields of related objects ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-task-update.md)
- [{#T}](./tasks-task-chat-message-send.md)
- [{#T}](./tasks-task-delete.md)