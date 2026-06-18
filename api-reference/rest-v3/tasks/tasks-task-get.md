# Get Task tasks.task.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.get` returns information about a task by its identifier.

Access to the data depends on permissions:
- Administrators can see all tasks,
- Managers can see their employees' tasks,
- Others can only see tasks available to them.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by using the old method of [getting the task list](../../tasks/tasks-task-list.md) ||
|| **select** 
[`array`](../../data-types.md) | An array of fields that the method will return. If `select` is not specified, a basic set of task fields without related objects is returned.

For related objects, use a nested path with a dot, for example `["responsible.name","responsible.email"]` - the response will include the `responsible` object with the requested fields. [List of fields with related objects](./fields.md)
||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TaskGetResult = {
      item: {
        id: number
        title: string
        description: string
        responsible?: {
          name: string
          email: string
        }
        deadline: ISODate | null
        status: string
        priority: string
        statusChanged: ISODate | null
        changed: ISODate | null
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<TaskGetResult>({
        method: 'tasks.task.get',
        params: {
          id: 8017,
          select: [
            'responsible.name',
            'responsible.email',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Task:', result.item.id, result.item.title, result.item.responsible)
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
      async function getTask() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'tasks.task.get',
            params: {
              id: 8017,
              select: [
                'responsible.name',
                'responsible.email',
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Task:', result.item.id, result.item.title, result.item.responsible)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTask)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

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
            "deadline": "2025-12-25T23:00:00+02:00",
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
            "statusChanged": "2025-11-24T06:00:00+02:00",
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
            "changed": "2025-12-10T16:04:54+02:00",
            "closed": null,
            "activity": "2025-12-10T16:04:42+02:00",
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
|| `id` | Required field `id` is missing | Add `id` to the request body ||
|| `id` | Field `id` requires data type `int` for this request | Ensure the value is a number, not a string ||
|#

#### Data Not Found Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `id` | Record with ID = `2` not found | Specify the `id` of an existing task ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Unknown field `status` for entity `TaskDto` | Specify existing fields in `select` for fetching fields of related objects ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-task-update.md)
- [{#T}](./tasks-task-chat-message-send.md)
- [{#T}](./tasks-task-delete.md)
