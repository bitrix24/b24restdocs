# Add Task tasks.task.add

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.add` adds a new task.

{% note info "" %}

Since version `tasks 25.700.0`, the method call is available in two API versions.

Old version:

`https://{installation_address}/rest/{user_id}/{rest_application_password}/tasks.task.add`

New version:

`https://{installation_address}/rest/api/{user_id}/{rest_application_password}/tasks.task.add`

Documentation in OpenApi format is available for the new version of the method call. To obtain OpenApi, call the method `documentation`:

`https://{installation_address}/rest/api/{user_id}/{rest_application_password}/documentation`

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../data-types.md) | Values of [task fields](./fields.md). Required fields for creating a task:
- `TITLE` — task title
- `RESPONSIBLE_ID` — responsible person's identifier

{% note warning "" %}

Check which required custom fields are configured for tasks in your Bitrix24. All required fields must be passed to the method.

{% endnote %}

You can pass the parameter `SE_PARAMETER` to the method — a list of objects with additional task parameters. Possible values for `CODE`:
- `1` — deadlines are determined by the deadlines of subtasks
- `2` — automatically complete the task when subtasks are completed (and vice versa)
- `3` — do not complete the task without a result

```js
SE_PARAMETER: [
    {
        VALUE: 'Y',
        CODE: 3
    },
    {
        VALUE: 'Y',
        CODE: 2
    }
]
```
||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

Let's add a task with files and CRM object bindings. To attach a file to the task, you need to add the character `n` before the file identifier.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TITLE":"Task Title","DEADLINE":"2025-12-31T23:59:59","CREATED_BY":456,"RESPONSIBLE_ID":123,"UF_CRM_TASK":["L_4","C_7","CO_5","D_10"],"UF_TASK_WEBDAV_FILES":["n12345","n67890"]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TITLE":"Task Title","DEADLINE":"2025-12-31T23:59:59","CREATED_BY":456,"RESPONSIBLE_ID":123,"UF_CRM_TASK":["L_4","C_7","CO_5","D_10"],"UF_TASK_WEBDAV_FILES":["n12345","n67890"]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            "tasks.task.add",
            {
                fields: {               
                    TITLE: "Task Title", // Task title
                    DEADLINE: "2025-12-31T23:59:59", // Deadline
                    CREATED_BY: 456, // Creator's identifier
                    RESPONSIBLE_ID: 123, // Assignee's identifier
                    // Example of passing multiple values in the UF_CRM_TASK field
                    UF_CRM_TASK: [
                        "L_4", // Link to lead
                        "C_7", // Link to contact
                        "CO_5", // Link to company
                        "D_10" // Link to deal
                    ],
                    // Example of passing multiple files in the UF_TASK_WEBDAV_FILES field
                    UF_TASK_WEBDAV_FILES: [
                        "n12345", // Identifier of the first disk file
                        "n67890" // Identifier of the second disk file
                    ]
                }
            }
        );
        
        const result = response.getData().result;
        console.info("Task successfully created with ID " + result.task.id);
    }
    catch( error )
    {
        console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.add',
                [
                    'fields' => [
                        'TITLE'         => 'Task Title',
                        'DEADLINE'      => '2025-12-31T23:59:59',
                        'CREATED_BY'    => 456,
                        'RESPONSIBLE_ID' => 123,
                        'UF_CRM_TASK'   => [
                            'L_4',
                            'C_7',
                            'CO_5',
                            'D_10',
                        ],
                        'UF_TASK_WEBDAV_FILES' => [
                            'n12345',
                            'n67890',
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Task successfully created with ID ' . $result['task']['id'];
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating task: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        "tasks.task.add",
        {
            fields: {               
                TITLE: "Task Title", // Task title
                DEADLINE: "2025-12-31T23:59:59", // Deadline
                CREATED_BY: 456, // Creator's identifier
                RESPONSIBLE_ID: 123, // Assignee's identifier
                // Example of passing multiple values in the UF_CRM_TASK field
                UF_CRM_TASK: [
                    "L_4", // Link to lead
                    "C_7", // Link to contact
                    "CO_5", // Link to company
                    "D_10" // Link to deal
                ],
                // Example of passing multiple files in the UF_TASK_WEBDAV_FILES field
                UF_TASK_WEBDAV_FILES: [
                    "n12345", // Identifier of the first disk file
                    "n67890" // Identifier of the second disk file
                ]
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info("Task successfully created with ID " + result.data().task.id);
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.add',
        [
            'fields' => [
                'TITLE' => 'Task Title', // Task title
                'DEADLINE' => '2025-12-31T23:59:59', // Deadline
                'CREATED_BY' => 456, // Creator's identifier
                'RESPONSIBLE_ID' => 123, // Assignee's identifier
                // Example of passing multiple values in the UF_CRM_TASK field
                'UF_CRM_TASK' => [
                    'L_4', // Link to lead
                    'C_7', // Link to contact
                    'CO_5', // Link to company
                    'D_10' // Link to deal
                ],
                // Example of passing multiple files in the UF_TASK_WEBDAV_FILES field
                'UF_TASK_WEBDAV_FILES' => [
                    'n12345', // Identifier of the first disk file
                    'n67890' // Identifier of the second disk file
                ]
            ]
        ]
    );

    if (isset($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Task successfully created with ID ' . $result['result']['task']['id'];
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "task": {
            "id": "3711",
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
            "createdDate": "2024-11-02T10:06:08+02:00",
            "responsibleId": "1",
            "changedBy": "1",
            "changedDate": "2024-11-02T10:06:08+02:00",
            "statusChangedBy": null,
            "closedBy": null,
            "closedDate": null,
            "activityDate": "2024-11-02T10:06:08+02:00",
            "dateStart": null,
            "deadline": null,
            "startDatePlan": null,
            "endDatePlan": null,
            "guid": "{c2794da9-c7fe-404d-a709-ddab4578717a}",
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
            "statusChangedDate": "2024-11-02T10:06:08+02:00",
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
    "time": {
        "start": 1758188171.142611,
        "finish": 1758188172.101309,
        "duration": 0.958698034286499,
        "processing": 0.9341180324554443,
        "date_start": "2025-09-18T12:36:11+03:00",
        "date_finish": "2025-09-18T12:36:12+03:00",
        "operating_reset_at": 1758188771,
        "operating": 0.9340989589691162
    }
}
```
### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Object with response data ||
|| **task**
[`object`](../data-types.md) | Object with [task description](./fields.md) after the operation is performed ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "Task title not specified\u003Cbr\u003E"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `100` | Could not find value for parameter {fields} (internal error) | Parameter `fields` not passed or empty ||
|| `ERROR_CORE` | User specified in the "Assignee" field not found | The identifier in the `RESPONSIBLE_ID` field refers to a non-existent user ||
|| `ERROR_CORE` | Assignee not specified | The `RESPONSIBLE_ID` field is not filled ||
|| `ERROR_CORE` | Task title not specified | The `TITLE` field is not filled ||
|| `ERROR_CORE` | Required field {field_name} value not entered | Required custom field with the specified name is not filled ||
|| `ERROR_CORE` | Invalid status | An incorrect value is specified in the `STATUS` field ||
|| `ERROR_CORE` | Task specified in the "Parent Task" field not found | The identifier in the `PARENT_ID` field refers to a non-existent task ||
|| `ERROR_CORE` | In deadline planning, the end date is earlier than the start date | The date and time in the `END_DATE_PLAN` field is earlier than in `START_DATE_PLAN` ||
|| `ERROR_CORE` | In deadline planning, the task duration is too long | The value in the `END_DATE_PLAN` field specifies a date that is too far in the future ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-task-update.md)
- [{#T}](./tasks-task-get.md)
- [{#T}](./tasks-task-list.md)
- [{#T}](./tasks-task-delete.md)
- [{#T}](./tasks-task-get-fields.md)
- [{#T}](../../tutorials/tasks/how-to-create-task-with-file.md)
- [{#T}](../../tutorials/tasks/how-to-connect-task-to-spa.md)
- [{#T}](../../tutorials/tasks/how-to-create-comment-with-file.md)