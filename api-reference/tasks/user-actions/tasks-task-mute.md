# Enable "Mute" Mode tasks.task.mute

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `tasks.task.mute` method turns on "Mute" mode in the task for the current user: notifications about task events stop arriving. To turn the mode off, use the [tasks.task.unmute](./tasks-task-unmute.md) method.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../data-types.md) | Task identifier. The method also accepts it as `taskId`.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or by using the [get task list method](../tasks-task-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TaskMuteResult = {
      task: {
        id: string
        isMuted: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<TaskMuteResult>({
        method: 'tasks.task.mute',
        params: {
          id: 8017,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Task muted, isMuted:', result.task.isMuted, 'taskId:', result.task.id)
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
      async function muteTask() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.task.mute',
            params: {
              id: 8017,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Task muted, isMuted:', result.task.isMuted, 'taskId:', result.task.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', muteTask)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.tasks.task.mute(
            bitrix_id=8017,
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "tasks.task.mute", b24.Params{
    	"id": 8017,
    })
    if err != nil {
    	return fmt.Errorf("tasks.task.mute: %w", err)
    }

    keys, ok := b24.Keys(res.Result)
    if !ok {
    	return fmt.Errorf("expected an object in the response")
    }
    fmt.Println("fields in response:", len(keys))
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
            "description": "",
            "mark": null,
            "priority": "1",
            "multitask": "N",
            "notViewed": "N",
            "replicate": "N",
            "stageId": "0",
            "sprintId": null,
            "backlogId": null,
            "createdBy": "503",
            "createdDate": "2026-11-19T19:48:49+01:00",
            "responsibleId": "503",
            "changedBy": "503",
            "changedDate": "2026-11-19T19:48:49+01:00",
            "statusChangedBy": null,
            "closedBy": null,
            "closedDate": null,
            "activityDate": "2026-11-19T19:48:49+01:00",
            "dateStart": null,
            "deadline": "2026-11-26T22:00:00+01:00",
            "startDatePlan": null,
            "endDatePlan": null,
            "guid": "{9915701d-b4b2-4f63-8ac1-801db88d1b6d}",
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
            "outlookVersion": "1",
            "viewedDate": null,
            "sorting": null,
            "durationFact": null,
            "isMuted": "Y",
            "isPinned": "N",
            "isPinnedInGroup": "N",
            "flowId": null,
            "chatId": 5961,
            "descriptionInBbcode": "Y",
            "status": "2",
            "statusChangedDate": "2026-11-19T19:48:49+01:00",
            "durationPlan": null,
            "durationType": "days",
            "favorite": "N",
            "groupId": "0",
            "auditors": [],
            "accomplices": [],
            "checklist": [],
            "group": [],
            "creator": {
                "id": "503",
                "name": "Maria Johnson",
                "link": "/company/personal/user/503/",
                "icon": "/bitrix/images/tasks/default_avatar.png",
                "workPosition": "Administrator"
            },
            "responsible": {
                "id": "503",
                "name": "Maria Johnson",
                "link": "/company/personal/user/503/",
                "icon": "/bitrix/images/tasks/default_avatar.png",
                "workPosition": "Administrator"
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
                "descendants": []
            },
            "checkListCanAdd": true
        }
    },
    "time": {
        "start": 1795106930,
        "finish": 1795106930.975382,
        "duration": 0.9753820896148682,
        "processing": 0,
        "date_start": "2026-11-19T19:48:50+01:00",
        "date_finish": "2026-11-19T19:48:50+01:00",
        "operating_reset_at": 1795107480,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with response data.

If the task with the specified `id` does not exist or the user has no access to it, the method does not return an error: the response contains an empty array `"result": []` ||
|| **task**
[`object`](../../data-types.md) | Object with [task description](../fields.md) after the operation is performed. The `isMuted` field is `Y` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"0",
    "error_description":"wrong task id"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for {taskId} | The `id` parameter is not provided ||
|| `400` | `0` | wrong task id | `id` is not a number, for example, `abc` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-task-unmute.md)
- [{#T}](../tasks-task-get.md)
