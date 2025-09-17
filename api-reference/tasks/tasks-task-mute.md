# Enable "Mute" Mode tasks.task.mute

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.mute` enables "Mute" mode for a task.

## Method Parameters

{% include [Note on parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by using the [get task list method](./tasks-task-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8017}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.mute
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":8017,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.mute
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.mute',
            {
                id: 8017,
            }
        );
        
        const result = response.getData().result;
        console.log('Muted task with ID:', result);
        
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
                'tasks.task.mute',
                [
                    'id' => 8017
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error muting task: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.task.mute',
        {
            'id': 8017
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
        'tasks.task.mute',
        [
            'id' => 8017
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "task": {
            "id": "8017",
            "parentId": "7817",
            "title": "Task Example",
            "description": "Task description with [B]formatting[/B]",
            "mark": "P",
            "priority": "2",
            "multitask": "N",
            "notViewed": "N",
            "replicate": "Y",
            "stageId": "395",
            "sprintId": null,
            "backlogId": null,
            "createdBy": "503",
            "createdDate": "2025-06-04T16:15:55+02:00",
            "responsibleId": "503",
            "changedBy": "503",
            "changedDate": "2025-09-10T15:22:12+02:00",
            "statusChangedBy": "503",
            "closedBy": "0",
            "closedDate": null,
            "activityDate": "2025-09-10T15:20:59+02:00",
            "dateStart": "2025-09-10T15:22:12+02:00",
            "deadline": "2025-09-11T08:00:00+02:00",
            "startDatePlan": null,
            "endDatePlan": null,
            "guid": "{21e7b78d-498e-4c84-a896-2dbeb26861b8}",
            "xmlId": null,
            "commentsCount": "43",
            "serviceCommentsCount": "27",
            "allowChangeDeadline": "Y",
            "allowTimeTracking": "Y",
            "taskControl": "N",
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
            "outlookVersion": "51",
            "viewedDate": "2025-09-10T15:21:58+02:00",
            "sorting": null,
            "durationFact": "1166",
            "isMuted": "Y",
            "isPinned": "N",
            "isPinnedInGroup": "N",
            "flowId": "19",
            "descriptionInBbcode": "Y",
            "status": "3",
            "statusChangedDate": "2025-09-10T15:22:12+02:00",
            "durationPlan": "0",
            "durationType": "days",
            "favorite": "Y",
            "groupId": "129",
            "auditors": [
                "61",
                "103",
                "547"
            ],
            "accomplices": [
                "3",
                "11"
            ],
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
                "name": "Workgroup",
                "opened": false,
                "membersCount": 1,
                "image": "\/bitrix\/images\/socialnetwork\/workgroup\/folder.png",
                "additionalData": []
            },
            "creator": {
                "id": "503",
                "name": "Maria Johnson",
                "link": "\/company\/personal\/user\/503\/",
                "icon": "https:\/\/mysite.com\/b17053\/resize_cache\/45749\/c0120a8d7c10d63c83e32398d1ec4d9e\/main\/c89\/c89c6b7301880958ea704b5a8470635c\/4R5A1256.png",
                "workPosition": "Administrator"
            },
            "responsible": {
                "id": "503",
                "name": "Maria Johnson",
                "link": "\/company\/personal\/user\/503\/",
                "icon": "https:\/\/mysite.com\/b17053\/resize_cache\/45749\/c0120a8d7c10d63c83e32398d1ec4d9e\/main\/c89\/c89c6b7301880958ea704b5a8470635c\/4R5A1256.png",
                "workPosition": "Administrator"
            },
            "accomplicesData": {
                "3": {
                    "id": "3",
                    "name": "Andrew Karpov",
                    "link": "\/company\/personal\/user\/3\/",
                    "icon": "https:\/\/mysite.com\/b17053\/resize_cache\/249\/c0120a8d7c10d63c83e32398d1ec4d9e\/main\/cd526b0644e7ff4d794ea41cb36bc423\/odmin.png",
                    "workPosition": "System Administrator"
                },
                "11": {
                    "id": "11",
                    "name": "Andrew Sergeyev",
                    "link": "\/company\/personal\/user\/11\/",
                    "icon": "https:\/\/mysite.com\/b17053\/resize_cache\/231\/c0120a8d7c10d63c83e32398d1ec4d9e\/main\/026bf59e161a0bd50f401d3796800651\/66b.jpg",
                    "workPosition": "Specialist"
                }
            },
            "auditorsData": {
                "61": {
                    "id": "61",
                    "name": "Ivan Petrov",
                    "link": "\/company\/personal\/user\/61\/",
                    "icon": "https:\/\/mysite.com\/b17053\/resize_cache\/8674\/c0120a8d7c10d63c83e32398d1ec4d9e\/main\/7b5\/7b52e4c2304ec0520dab3d4261e9ca1f\/sp.jpg",
                    "workPosition": "Marketer"
                },
                "103": {
                    "id": "103",
                    "name": "Svetlana Ivanova",
                    "link": "\/company\/personal\/user\/103\/",
                    "icon": "https:\/\/mysite.com\/b17053\/resize_cache\/8644\/c0120a8d7c10d63c83e32398d1ec4d9e\/main\/45f\/45fff10d17d398a5583184c8350cd197\/buh.jpg",
                    "workPosition": "Accountant"
                },
                "547": {
                    "id": "547",
                    "name": "Maria",
                    "link": "\/company\/personal\/user\/547\/",
                    "icon": "\/bitrix\/images\/tasks\/default_avatar.png",
                    "workPosition": "Tester"
                }
            },
            "newCommentsCount": 0,
            "action": {
                "accept": false,
                "decline": false,
                "complete": true,
                "approve": false,
                "disapprove": false,
                "start": false,
                "pause": true,
                "delegate": true,
                "remove": true,
                "edit": true,
                "defer": false,
                "renew": false,
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
                "dayplan.timer.toggle": true,
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
            "start": 1757513814.799823,
            "finish": 1757513814.898483,
            "duration": 0.09865999221801758,
            "processing": 0.07469487190246582,
            "date_start": "2025-09-10T17:16:54+02:00",
            "date_finish": "2025-09-10T17:16:54+02:00",
            "operating_reset_at": 1757514414,
            "operating": 0
        }
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Object with response data.

Returns an empty array `"result":[]` if the user does not have access to the task or if the task with the specified `id` does not exist ||
|| **task**
[`object`](../data-types.md) | Object with [task description](./fields.md) after the operation is performed ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"0",
    "error_description":"wrong task id"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | wrong task id | The value in the `id` parameter is of an incorrect type ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./tasks-task-unmute.md)