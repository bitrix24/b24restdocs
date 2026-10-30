# Update Task tasks.task.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: 
> - any user with access to edit the task
> - task Creator

The method `tasks.task.update` changes task fields and returns the full task after the update.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **taskId***
[`integer`](../data-types.md) | Task identifier. 

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by using the [get task list method](./tasks-task-list.md) ||
|| **fields***
[`object`](../data-types.md) | Values of the [task fields](./fields.md) to change. Pass only the fields you are changing: fields that are not passed retain their current values. The method ignores unknown fields. If you pass an empty object `{}`, the method changes nothing and returns the task.

You can pass the `SE_PARAMETER` parameter to the method — a list of objects with additional task parameters. Possible values for the `CODE` codes:
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

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":11,"fields":{"UF_CRM_TASK":["L_4","C_7","CO_5","D_10"],"UF_TASK_WEBDAV_FILES":["n12345","n67890"],"RESPONSIBLE_ID":123}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":11,"fields":{"UF_CRM_TASK":["L_4","C_7","CO_5","D_10"],"UF_TASK_WEBDAV_FILES":["n12345","n67890"],"RESPONSIBLE_ID":123},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Minimal shape of result.task; the API returns the full task object
    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TaskUpdateResult = {
      task: { id: string }
    }

    try {
      const response = await $b24.actions.v2.call.make<TaskUpdateResult>({
        method: 'tasks.task.update',
        params: {
          taskId: 11, // ID of the task to update
          fields: {
            // Bind the task to several CRM entities via UF_CRM_TASK
            UF_CRM_TASK: ['L_4', 'C_7', 'CO_5', 'D_10'], // Lead 4 / Contact 7 / Company 5 / Deal 10
            // Attach several Drive files via UF_TASK_WEBDAV_FILES (prefix IDs with "n")
            UF_TASK_WEBDAV_FILES: ['n12345', 'n67890'],
            RESPONSIBLE_ID: 123 // New responsible person ID
          }
        },
        requestId: Text.getUuidRfc4122() // optional unique tracking id for this request
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(`Task ${result.task.id} updated`)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function updateTask() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.task.update',
            params: {
              taskId: 11, // ID of the task to update
              fields: {
                // Bind the task to several CRM entities via UF_CRM_TASK
                UF_CRM_TASK: ['L_4', 'C_7', 'CO_5', 'D_10'], // Lead 4 / Contact 7 / Company 5 / Deal 10
                // Attach several Drive files via UF_TASK_WEBDAV_FILES (prefix IDs with "n")
                UF_TASK_WEBDAV_FILES: ['n12345', 'n67890'],
                RESPONSIBLE_ID: 123 // New responsible person ID
              }
            },
            requestId: B24Js.Text.getUuidRfc4122() // optional unique tracking id for this request
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(`Task ${result.task.id} updated`)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateTask)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "UF_CRM_TASK": [
            "L_4",
            "C_7",
            "CO_5",
            "D_10",
        ],
        "UF_TASK_WEBDAV_FILES": [
            "n12345",
            "n67890",
        ],
        "RESPONSIBLE_ID": 123,
    }

    try:
        bitrix_response = client.tasks.task.update(
            task_id=11,
            fields=fields,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.update',
                [
                    'taskId' => 11, // Identifier of the task you want to update
                    'fields' => [
                        // Example of passing multiple values in the UF_CRM_TASK field
                        'UF_CRM_TASK' => [
                            'L_4', // Link to lead with id 4
                            'C_7', // Link to contact with id 7
                            'CO_5', // Link to company with id 5
                            'D_10' // Link to deal with id 10
                        ],
                        // Example of passing multiple files in the UF_TASK_WEBDAV_FILES field
                        'UF_TASK_WEBDAV_FILES' => [
                            'n12345', // Identifier of the first disk file
                            'n67890' // Identifier of the second disk file
                        ],
                        'RESPONSIBLE_ID' => 123 // Identifier of the new Participant
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Task successfully updated';
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating task: ' . $e->getMessage();
    }
    ```

- BX24.js

   ```javascript
    BX24.callMethod(
        "tasks.task.update",
        {
            taskId: 11, // Identifier of the task you want to update
            fields: {
                // Example of passing multiple values in the UF_CRM_TASK field
                UF_CRM_TASK: [
                    "L_4", // Link to lead with id 4
                    "C_7", // Link to contact with id 7
                    "CO_5", // Link to company with id 5
                    "D_10" // Link to deal with id 10
                ],
                // Example of passing multiple files in the UF_TASK_WEBDAV_FILES field
                UF_TASK_WEBDAV_FILES: [
                    "n12345", // Identifier of the first disk file
                    "n67890" // Identifier of the second disk file
                ],
                RESPONSIBLE_ID: 123 // Identifier of the new Participant
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info("Task successfully updated");
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.update',
        [
            'taskId' => 11, // Identifier of the task you want to update
            'fields' => [
                // Example of passing multiple values in the UF_CRM_TASK field
                'UF_CRM_TASK' => [
                    'L_4', // Link to lead with id 4
                    'C_7', // Link to contact with id 7
                    'CO_5', // Link to company with id 5
                    'D_10' // Link to deal with id 10
                ],
                // Example of passing multiple files in the UF_TASK_WEBDAV_FILES field
                'UF_TASK_WEBDAV_FILES' => [
                    'n12345', // Identifier of the first disk file
                    'n67890' // Identifier of the second disk file
                ],
                'RESPONSIBLE_ID' => 123 // Identifier of the new Participant
            ]
        ]
    );

    if (isset($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Task successfully updated';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "tasks.task.update", b24.Params{
    	"taskId": 11,
    	"fields": b24.Params{
    		"UF_CRM_TASK":          []string{"L_4", "C_7", "CO_5", "D_10"},
    		"UF_TASK_WEBDAV_FILES": []string{"n12345", "n67890"},
    		"RESPONSIBLE_ID":       123,
    	},
    })
    if err != nil {
    	return fmt.Errorf("tasks.task.update: %w", err)
    }

    // The method wraps the response in an object with the "task" key.
    raw, ok := b24.Unwrap(res.Result, "task")
    if !ok {
    	return fmt.Errorf("no task key in the response")
    }

    var item struct {
    	ID          b24.ID `json:"id"`
    	Title       string `json:"title"`
    	Description string `json:"description"`
    	Priority    string `json:"priority"`
    	Multitask   string `json:"multitask"`
    	NotViewed   string `json:"notViewed"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Title)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "task": {
            "id": "11",
            "parentId": null,
            "title": "Task Example",
            "description": "Task description",
            "mark": null,
            "priority": "1",
            "multitask": "N",
            "notViewed": "N",
            "replicate": "N",
            "stageId": "0",
            "sprintId": null,
            "backlogId": null,
            "createdBy": "1",
            "createdDate": "2026-10-29T15:35:23+01:00",
            "responsibleId": "1",
            "changedBy": "1",
            "changedDate": "2026-10-29T15:35:24+01:00",
            "statusChangedBy": null,
            "closedBy": null,
            "closedDate": null,
            "activityDate": "2026-10-29T15:35:24+01:00",
            "dateStart": null,
            "deadline": "2026-12-31T23:59:00+01:00",
            "startDatePlan": null,
            "endDatePlan": null,
            "guid": "{aa8a90e4-69a4-453a-b384-76f1a95569a1}",
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
            "subordinate": "N",
            "exchangeModified": null,
            "exchangeId": null,
            "outlookVersion": "2",
            "viewedDate": null,
            "sorting": null,
            "durationFact": null,
            "isMuted": "N",
            "isPinned": "N",
            "isPinnedInGroup": "N",
            "flowId": null,
            "chatId": 5747,
            "descriptionInBbcode": "Y",
            "status": "2",
            "statusChangedDate": "2026-10-29T15:35:24+01:00",
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
                "name": "Klaus Weber",
                "link": "/company/personal/user/1/",
                "icon": "https://test.bitrix24.com/upload/resize_cache/main/avatar.png",
                "workPosition": null
            },
            "responsible": {
                "id": "1",
                "name": "Klaus Weber",
                "link": "/company/personal/user/1/",
                "icon": "https://test.bitrix24.com/upload/resize_cache/main/avatar.png",
                "workPosition": null
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
                    "attachments": [],
                    "nodeId": null
                },
                "action": [],
                "descendants": []
            },
            "checkListCanAdd": true
        }
    },
    "time": {
        "start": 1793277324,
        "finish": 1793277324.579209,
        "duration": 0.5792090892791748,
        "processing": 0,
        "date_start": "2026-10-29T15:35:24+01:00",
        "date_finish": "2026-10-29T15:35:24+01:00",
        "operating_reset_at": 1793277900,
        "operating": 0.20716381072998047
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response ||
|| **task**
[`object`](../data-types.md) | All task fields after the update, including those that were not passed. [Task field description](./fields.md) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "Cannot link node to itself\u003Cbr\u003E"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | wrong task id | The `taskId` parameter is not a number ||
|| `400` | `100` | Could not find value for {taskId} | The required `taskId` parameter is not provided ||
|| `400` | `0` | This task action is not allowed | The task with the specified `taskId` was not found, or the user does not have permission to change the task ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter is not provided ||
|| `400` | `ERROR_CORE` | User specified as assignee was not found.\u003Cbr\u003E | The `RESPONSIBLE_ID` field contains the identifier of a non-existent user ||
|| `400` | `ERROR_CORE` | Incorrect status\u003Cbr\u003E | The `STATUS` field contains a value not in the list `1`, `2`, `3`, `4`, `5`, `6`. In the current version, the method may not return this error: an invalid value is ignored, and the status remains unchanged ||
|| `400` | `ERROR_CORE` | A task specified in the field \u0026quot;Subtask\u0026quot; was not found.\u003Cbr\u003E | The `PARENT_ID` field contains the identifier of a non-existent task ||
|| `400` | `ERROR_CORE` | Cannot link node to itself\u003Cbr\u003E | The `PARENT_ID` field contains the identifier of the same task as in the `taskId` parameter ||
|| `400` | `ERROR_CORE` | Tasks cannot use circular references.\u003Cbr\u003E | The `PARENT_ID` field contains the identifier of a subtask `taskId`. A subtask cannot become a parent task ||
|| `400` | `ERROR_CORE` | The planned end date is earlier than the start date.\u003Cbr\u003E | The date and time in the `END_DATE_PLAN` field is earlier than in `START_DATE_PLAN` ||
|| `400` | `ERROR_CORE` | The planned task duration period is too long\u003Cbr\u003E | The value in the `END_DATE_PLAN` field specifies a date that is too far in the future ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-task-update-rest-v3.md)
- [{#T}](./tasks-task-add.md)
- [{#T}](./tasks-task-get.md)
- [{#T}](./tasks-task-list.md)
- [{#T}](./tasks-task-delete.md)
- [{#T}](./tasks-task-get-fields.md)
- [{#T}](../../tutorials/tasks/how-to-upload-file-to-task.md)
- [{#T}](../../tutorials/tasks/how-to-connect-task-to-spa.md)
- [{#T}](../../tutorials/tasks/how-to-create-comment-with-file.md)