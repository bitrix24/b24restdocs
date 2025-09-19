# Approve Task tasks.task.approve

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: task creator or administrator

The method `tasks.task.approve` accepts the work of the assignee on a task when task control is enabled and changes the task status to Completed.

You can check the permission to accept the work of the assignee on the task using the [check access to the task](./tasks-task-get-access.md) method.

## Method Parameters

{% include [Note on parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **taskId***
[`integer`](../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or using the [get task list](./tasks-task-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8017}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.approve
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8017,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.approve
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.approve',
            {
                taskId: 8017,
            }
        );
        
        const result = response.getData().result;
        console.log('Approved task with ID:', result);
        
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.approve',
                [
                    'taskId' => 8017
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error approving task: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.task.approve',
        {
            'taskId': 8017
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.approve',
        [
            'taskId' => 8017
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
        "task": {
            "id": "8017",
            "parentId": null,
            "title": "Task Example",
            "description": "Task description with [B]formatting[/B]",
            "mark": "P",
            "priority": "2",
            "multitask": "N",
            "notViewed": "N",
            "replicate": "Y",
            "stageId": "397",
            "sprintId": null,
            "backlogId": null,
            "createdBy": "503",
            "createdDate": "2025-06-04T16:15:55+02:00",
            "responsibleId": "547",
            "changedBy": "503",
            "changedDate": "2025-09-18T12:36:11+02:00",
            "statusChangedBy": "503",
            "closedBy": "503",
            "closedDate": "2025-09-18T12:36:11+02:00",
            "activityDate": "2025-09-18T12:36:11+02:00",
            "dateStart": "2025-09-10T15:22:12+02:00",
            "deadline": "2025-09-11T08:00:00+02:00",
            "startDatePlan": null,
            "endDatePlan": null,
            "guid": "{21e7b78d-498e-4c84-a896-2dbeb26861b8}",
            "xmlId": null,
            "commentsCount": "62",
            "serviceCommentsCount": "44",
            "allowChangeDeadline": "Y",
            "allowTimeTracking": "Y",
            "taskControl": "Y",
            "addInReport": "N",
            "forkedByTemplateId": null,
            "timeEstimate": "101580",
            "timeSpentInLogs": "69963",
            "matchWorkTime": "Y",
            "forumTopicId": "1273",
            "forumId": "11",
            "siteId": "s1",
            "subordinate": "Y",
            "exchangeModified": null,
            "exchangeId": null,
            "outlookVersion": "87",
            "viewedDate": "2025-09-18T12:35:22+02:00",
            "sorting": null,
            "durationFact": "1166",
            "isMuted": "N",
            "isPinned": "N",
            "isPinnedInGroup": "N",
            "flowId": null,
            "descriptionInBbcode": "Y",
            "status": "5",
            "statusChangedDate": "2025-09-18T12:36:11+02:00",
            "durationPlan": "0",
            "durationType": "days",
            "favorite": "Y",
            "groupId": "129",
            "auditors": [
                "103",
                "503"
            ],
            "accomplices": [],
            "checklist": {
                "433": {
                    "id": "433",
                    "taskId": "8017",
                    "createdBy": "503",
                    "parentId": "431",
                    "title": "First",
                    "sortIndex": "0",
                    "isComplete": "N",
                    "isImportant": "N",
                    "toggledBy": null,
                    "toggledDate": null,
                    "ufChecklistFiles": false,
                    "members": [],
                    "attachments": [],
                    "entityId": "8017",
                    "action": {
                        "modify": true,
                        "remove": true,
                        "toggle": true,
                        "add": true,
                        "addAccomplice": true
                    }
                },
                "431": {
                    "id": "431",
                    "taskId": "8017",
                    "createdBy": "503",
                    "parentId": 0,
                    "title": "Checklist 1",
                    "sortIndex": "0",
                    "isComplete": "N",
                    "isImportant": "N",
                    "toggledBy": null,
                    "toggledDate": null,
                    "ufChecklistFiles": false,
                    "members": [],
                    "attachments": [],
                    "entityId": "8017",
                    "action": {
                        "modify": true,
                        "remove": true,
                        "toggle": true,
                        "add": true,
                        "addAccomplice": true
                    }
                },
                "435": {
                    "id": "435",
                    "taskId": "8017",
                    "createdBy": "503",
                    "parentId": "431",
                    "title": "Second",
                    "sortIndex": "1",
                    "isComplete": "N",
                    "isImportant": "N",
                    "toggledBy": null,
                    "toggledDate": null,
                    "ufChecklistFiles": false,
                    "members": [],
                    "attachments": [],
                    "entityId": "8017",
                    "action": {
                        "modify": true,
                        "remove": true,
                        "toggle": true,
                        "add": true,
                        "addAccomplice": true
                    }
                }
            },
            "group": {
                "id": "129",
                "name": "New Flow",
                "opened": false,
                "membersCount": 1,
                "image": "/bitrix/images/socialnetwork/workgroup/folder.png",
                "additionalData": []
            },
            "creator": {
                "id": "503",
                "name": "Maria Ivanova",
                "link": "/company/personal/user/503/",
                "icon": "https://mysite.com/b17053/resize_cache/45749/c0120a8d7c10d63c83e32398d1ec4d9e/main/c89/c89c6b7301880958ea704b5a8470635c/4R5A1256.png",
                "workPosition": "Administrator"
            },
            "responsible": {
                "id": "547",
                "name": "Maria",
                "link": "/company/personal/user/547/",
                "icon": "/bitrix/images/tasks/default_avatar.png",
                "workPosition": "Tester"
            },
            "accomplicesData": [],
            "auditorsData": {
                "103": {
                    "id": "103",
                    "name": "Svetlana Ivanova",
                    "link": "/company/personal/user/103/",
                    "icon": "https://mysite.com/b17053/resize_cache/8644/c0120a8d7c10d63c83e32398d1ec4d9e/main/45f/45fff10d17d398a5583184c8350cd197/buh.jpg",
                    "workPosition": "Accountant"
                },
                "503": {
                    "id": "503",
                    "name": "Maria Ivanova",
                    "link": "/company/personal/user/503/",
                    "icon": "https://mysite.com/b17053/resize_cache/45749/c0120a8d7c10d63c83e32398d1ec4d9e/main/c89/c89c6b7301880958ea704b5a8470635c/4R5A1256.png",
                    "workPosition": "Administrator"
                }
            },
            "newCommentsCount": 2,
            "action": {
                "accept": false,
                "decline": false,
                "complete": false,
                "approve": false,
                "disapprove": false,
                "start": false,
                "pause": false,
                "delegate": false,
                "remove": true,
                "edit": true,
                "defer": false,
                "renew": true,
                "create": true,
                "changeDeadline": true,
                "checklistAddItems": true,
                "addFavorite": false,
                "deleteFavorite": true,
                "rate": true,
                "take": false,
                "edit.originator": false,
                "checklist.reorder": true,
                "elapsedtime.add": true,
                "dayplan.timer.toggle": false,
                "edit.plan": true,
                "checklist.add": true,
                "favorite.add": false,
                "favorite.delete": true
            },
            "checkListTree": {
                "nodeId": 0,
                "fields": {
                    "id": null,
                    "copiedId": null,
                    "entityId": null,
                    "userId": 503,
                    "createdBy": null,
                    "parentId": null,
                    "title": "",
                    "sortIndex": null,
                    "displaySortIndex": "",
                    "isComplete": false,
                    "isImportant": false,
                    "completedCount": 0,
                    "members": [],
                    "attachments": [],
                    "nodeId": null
                },
                "action": [],
                "descendants": [
                    {
                        "nodeId": 1,
                        "fields": {
                            "id": 431,
                            "copiedId": null,
                            "entityId": 8017,
                            "userId": 503,
                            "createdBy": 503,
                            "parentId": 0,
                            "title": "Checklist 1",
                            "sortIndex": 0,
                            "displaySortIndex": "",
                            "isComplete": false,
                            "isImportant": false,
                            "completedCount": 0,
                            "members": [],
                            "attachments": [],
                            "nodeId": null
                        },
                        "action": {
                            "modify": true,
                            "remove": true,
                            "toggle": true,
                            "add": true,
                            "addAccomplice": true
                        },
                        "descendants": [
                            {
                                "nodeId": 2,
                                "fields": {
                                    "id": 433,
                                    "copiedId": null,
                                    "entityId": 8017,
                                    "userId": 503,
                                    "createdBy": 503,
                                    "parentId": 431,
                                    "title": "First",
                                    "sortIndex": 0,
                                    "displaySortIndex": "1",
                                    "isComplete": false,
                                    "isImportant": false,
                                    "completedCount": 0,
                                    "members": [],
                                    "attachments": [],
                                    "nodeId": null
                                },
                                "action": {
                                    "modify": true,
                                    "remove": true,
                                    "toggle": true,
                                    "add": true,
                                    "addAccomplice": true
                                },
                                "descendants": []
                            },
                            {
                                "nodeId": 3,
                                "fields": {
                                    "id": 435,
                                    "copiedId": null,
                                    "entityId": 8017,
                                    "userId": 503,
                                    "createdBy": 503,
                                    "parentId": 431,
                                    "title": "Second",
                                    "sortIndex": 1,
                                    "displaySortIndex": "2",
                                    "isComplete": false,
                                    "isImportant": false,
                                    "completedCount": 0,
                                    "members": [],
                                    "attachments": [],
                                    "nodeId": null
                                },
                                "action": {
                                    "modify": true,
                                    "remove": true,
                                    "toggle": true,
                                    "add": true,
                                    "addAccomplice": true
                                },
                                "descendants": []
                            }
                        ]
                    }
                ]
            },
            "checkListCanAdd": true
        },
        "time": {
            "start": 1758188171.142611,
            "finish": 1758188172.101309,
            "duration": 0.958698034286499,
            "processing": 0.9341180324554443,
            "date_start": "2025-09-18T12:36:11+02:00",
            "date_finish": "2025-09-18T12:36:12+02:00",
            "operating_reset_at": 1758188771,
            "operating": 0.9340989589691162
        }
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

HTTP Status: **400**

```json
{
    "error":"1048582",
    "error_description":"Action on the task is not allowed"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | wrong task id | The `taskId` parameter has an incorrect type ||
|| `1048582` | Action on the task is not allowed | The user does not have permission to accept work on the task ||
|| `100` | CTaskItem All parameters in the constructor must have real class type | The required parameter `taskId` is not specified ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-task-disapprove.md)
- [{#T}](./tasks-task-get-access.md)