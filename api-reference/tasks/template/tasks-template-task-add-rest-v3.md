# Create a Task from a Template v 3.0 tasks.template.task.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the template

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `tasks.template.task.add` method creates a task from a template and returns it.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../data-types.md) | Task template ID.

The ID can be retrieved with the [tasks.template.add](./tasks-template-add-rest-v3.md) method when creating a template or with the [tasks.template.list](./tasks-template-list-rest-v3.md) method ||
|| **fields***
[`object`](../../data-types.md) | Task fields that override the template values [(detailed description)](#fields). The task receives the other fields from the template.

To create a task from the template only, pass an empty object `{}` ||
|| **withSubTasks**
[`boolean`](../../data-types.md) | Whether to create subtasks from child templates.

Possible values:

- `true` — create subtasks from child templates of all levels
- `false` — create the task only

Default: `false` ||
|| **view**
[`boolean`](../../data-types.md) | Whether to mark the task as viewed by the user who called the method.

Possible values:

- `true` — the task is marked as viewed immediately
- `false` — the task is created as unviewed

Default: `false` ||
|#

### Parameter fields {#fields}

Only the listed fields can be passed in `fields`. The format and values of the fields are in the [task field description](../fields-rest-v3.md#taskdto).

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../data-types.md) | Task title. An empty string causes an error ||
|| **description**
[`string`](../../data-types.md) | Task description ||
|| **responsibleId**
[`integer`](../../data-types.md) | Responsible person ID.

The user ID can be retrieved with the [user.get](../../user/user-get.md) method ||
|| **deadline**
[`datetime`](../../data-types.md) | Deadline, for example, `2026-12-31T16:00:00+01:00` ||
|| **startPlan**
[`datetime`](../../data-types.md) | Planned start date ||
|| **endPlan**
[`datetime`](../../data-types.md) | Planned end date ||
|| **priority**
[`string`](../../data-types.md) | Task priority.

Possible values:

- `high` — high
- `average` — medium
- `low` — low ||
|| **groupId**
[`integer`](../../data-types.md) | Group or project ID.

The ID can be retrieved with the [sonet_group.get](../../sonet-group/sonet-group-get.md) method ||
|| **stageId**
[`integer`](../../data-types.md) | ID of the group Kanban stage ||
|| **flowId**
[`integer`](../../data-types.md) | Flow ID ||
|| **epicId**
[`integer`](../../data-types.md) | Scrum epic ID ||
|| **storyPoints**
[`integer`](../../data-types.md) | Story points of the Scrum task ||
|| **parentId**
[`integer`](../../data-types.md) | Parent task ID ||
|| **crmItemIds**
[`string[]`](../../data-types.md) | CRM item codes, for example, `["D_9193","L_1001345"]`. The code format is in the [task field description](../fields-rest-v3.md#taskdto) ||
|| **fileIds**
[`integer[]`](../../data-types.md) | IDs of Drive files to attach to the task ||
|| **checklist**
[`array`](../../data-types.md) | Task checklist ||
|| **reminders**
[`array`](../../data-types.md) | Task reminders ||
|| **dependsOn**
[`array`](../../data-types.md) | Task dependencies ||
|| **needsControl**
[`boolean`](../../data-types.md) | Task review by the creator: after the responsible person completes the task, it awaits review ||
|| **requireResult**
[`boolean`](../../data-types.md) | Require a result: the task cannot be completed without a result record ||
|| **plannedDuration**
[`integer`](../../data-types.md) | Planned task duration ||
|| **durationType**
[`string`](../../data-types.md) | Unit of the planned duration.

Possible values: `seconds`, `minutes`, `hours`, `days`, `weeks`, `months`, `years` ||
|| **estimatedTime**
[`integer`](../../data-types.md) | Time estimate in seconds ||
|| **allowsChangeDeadline**
[`boolean`](../../data-types.md) | Allow the responsible person to change the deadline ||
|| **allowsChangeDatePlan**
[`boolean`](../../data-types.md) | Allow the responsible person to change the planned dates ||
|| **maxDeadlineChangeDate**
[`datetime`](../../data-types.md) | Date after which the responsible person cannot postpone the deadline ||
|| **maxDeadlineChanges**
[`integer`](../../data-types.md) | Maximum number of deadline postponements ||
|| **requireDeadlineChangeReason**
[`boolean`](../../data-types.md) | Require a reason when postponing the deadline ||
|| **allowsTimeTracking**
[`boolean`](../../data-types.md) | Enable time tracking for the task ||
|| **matchesWorkTime**
[`boolean`](../../data-types.md) | Consider working hours: skip weekends when calculating planned dates ||
|| **matchesSubTasksTime**
[`boolean`](../../data-types.md) | Consider subtask dates when calculating planned dates ||
|| **autocompleteSubTasks**
[`boolean`](../../data-types.md) | Complete subtasks together with the task ||
|| **checklistAutonumbering**
[`boolean`](../../data-types.md) | Number checklist items. If the field is not passed, the value is taken from the template ||
|| **addInReport**
[`boolean`](../../data-types.md) | Add the task to the report ||
|| **isMultitask**
[`boolean`](../../data-types.md) | "Base task with subtasks" flag ||
|| **siteId**
[`string`](../../data-types.md) | Site ID, for example, `s1` ||
|#

The method does not accept other task fields, for example, `creatorId`, `accomplices`, `auditors`, `tags`, and custom fields, and returns the `Field is not allowed to write` error. The task receives the creator, participants, auditors, and tags from the template.

If the template has several responsible persons, the method creates a task for each of them. With `withSubTasks: true`, the method creates no more than 200 tasks per call, including subtasks and copies for each responsible person. If the template produces more, the method creates no tasks at all.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.task.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":125,"fields":{"title":"Prepare a report","deadline":"2026-12-31T16:00:00+01:00","priority":"high"},"withSubTasks":true,"view":true}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.task.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":125,"fields":{"title":"Prepare a report","deadline":"2026-12-31T16:00:00+01:00","priority":"high"},"withSubTasks":true,"view":true,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.task.add
    ```

- JS (TS)

    REST 3.0 task template methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TemplateTaskAddResult = {
      item: {
        id: number
        title: string
        creatorId: number
        responsibleId: number
        deadline: ISODate | null
        priority: string
        status: string
        containsSubTasks: boolean
        link: string
        crmItemIds: string[]
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateTaskAddResult>(
        'tasks.template.task.add',
        {
          templateId: 125,
          fields: {
            title: 'Prepare a report',
            deadline: '2026-12-31T16:00:00+01:00',
            priority: 'high',
          },
          withSubTasks: true,
          view: true,
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Task created:', result.item.id, result.item.title, result.item.status)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    REST 3.0 task template methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function addTaskFromTemplate() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.task.add',
            {
              templateId: 125,
              fields: {
                title: 'Prepare a report',
                deadline: '2026-12-31T16:00:00+01:00',
                priority: 'high',
              },
              withSubTasks: true,
              view: true,
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Task created:', result.item.id, result.item.title, result.item.status)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addTaskFromTemplate)
    </script>
    ```

- Python

    REST 3.0 task template methods are not yet included in the `Config().api_v3_methods` list, and without it `b24pysdk` sends the call to the old REST address. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.task.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "templateId": 125,
                "fields": {
                    "title": "Prepare a report",
                    "deadline": "2026-12-31T16:00:00+01:00",
                    "priority": "high",
                },
                "withSubTasks": True,
                "view": True,
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.template.task.add',
                [
                    'templateId' => 125,
                    'fields' => [
                        'title' => 'Prepare a report',
                        'deadline' => '2026-12-31T16:00:00+01:00',
                        'priority' => 'high',
                    ],
                    'withSubTasks' => true,
                    'view' => true,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'tasks.template.task.add',
        {
            templateId: 125,
            fields: {
                title: 'Prepare a report',
                deadline: '2026-12-31T16:00:00+01:00',
                priority: 'high',
            },
            withSubTasks: true,
            view: true,
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.template.task.add',
        [
            'templateId' => 125,
            'fields' => [
                'title' => 'Prepare a report',
                'deadline' => '2026-12-31T16:00:00+01:00',
                'priority' => 'high',
            ],
            'withSubTasks' => true,
            'view' => true,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "tasks.template.task.add", b24.Params{
    	"templateId": 125,
    	"fields": b24.Params{
    		"title":    "Prepare a report",
    		"deadline": "2026-12-31T16:00:00+01:00",
    		"priority": "high",
    	},
    	"withSubTasks": true,
    	"view":         true,
    })
    if err != nil {
    	return fmt.Errorf("tasks.template.task.add: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var task struct {
    	ID     b24.ID `json:"id"`
    	Title  string `json:"title"`
    	Status string `json:"status"`
    }
    if err := json.Unmarshal(raw, &task); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(task.ID, task.Title)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

The method returns the created task. Subtasks created with `withSubTasks: true` are not included in the response; their presence is indicated by `containsSubTasks: true`. Subtasks can be retrieved with the [tasks.task.list](../tasks-task-list-rest-v3.md) method with a filter by `parentId`.

```json
{
    "result": {
        "item": {
            "id": 4403,
            "title": "Prepare a report",
            "description": "Prepare a report on the week's results",
            "creatorId": 1269,
            "responsibleId": 1269,
            "deadline": "2026-12-31T16:00:00+01:00",
            "needsControl": false,
            "startPlan": null,
            "endPlan": null,
            "fileIds": null,
            "checklist": [],
            "epicId": null,
            "storyPoints": null,
            "priority": "high",
            "status": "pending",
            "statusChanged": null,
            "parentId": null,
            "containsChecklist": false,
            "containsSubTasks": true,
            "containsRelatedTasks": false,
            "containsGanttLinks": false,
            "containsPlacements": true,
            "containsResults": false,
            "numberOfReminders": 0,
            "chatId": 6167,
            "plannedDuration": 0,
            "actualDuration": 0,
            "durationType": "days",
            "started": null,
            "estimatedTime": 0,
            "replicate": null,
            "changed": "2026-11-23T11:18:08+01:00",
            "changedById": 1269,
            "closed": null,
            "activity": "2026-11-23T11:18:08+01:00",
            "guid": "{6d42e14e-53f4-49cf-b896-e6fdd0912c20}",
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
            "link": "/company/personal/user/1269/tasks/task/view/4403/",
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
            "archiveLink": "/bitrix/tools/disk/uf.php?entityId=4403&entity=TASKS_TASK&fieldName=UF_TASK_WEBDAV_FILES&signature=332019922b4f031c5ca78974acb2bc735b650581d2b7d8e9a4ca354ca549c12c&action=downloadArchiveByEntity&ncc=1",
            "crmItemIds": [
                "D_9193",
                "C_4575",
                "CO_2985",
                "L_1001345",
                "SI_117"
            ],
            "requireResult": false,
            "checklistAutonumbering": false,
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
        "start": 1795429088,
        "finish": 1795429089.49397,
        "duration": 1.4939699172973633,
        "processing": 1,
        "date_start": "2026-11-23T11:18:08+01:00",
        "date_finish": "2026-11-23T11:18:09+01:00",
        "operating_reset_at": 1795429680,
        "operating": 0.9652161598205566
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
[`object`](../../data-types.md) | The created task. [Task field description](../fields-rest-v3.md#taskdto) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**, **404**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "Field is not allowed to write",
                "field": "creatorId"
            },
            {
                "message": "Field is not allowed to write",
                "field": "auditors"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `templateId` | Required field `templateId` is missing | Add `templateId` to the request body ||
|| `templateId` | This request requires data type `int` in field `templateId` | Pass the template ID as a number ||
|| `fields` | Required field `fields` is missing | Add `fields` to the request body. To create a task from the template only, pass `{}` ||
|| Field name from `fields` | Field is not allowed to write | Remove the field from `fields`: it cannot be overridden. The list of allowed fields is in the [fields parameter description](#fields) ||
|#

#### Task Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | The task name is not specified | Pass a non-empty `title` or omit the field to take the title from the template ||
|| `-` | `User specified as assignee was not found` | Check `responsibleId` with the [user.get](../../user/user-get.md) method ||
|#

#### Unknown Field

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Unknown field `foo` found in entity `TaskDto` | Check the field names against the [fields parameter description](#fields) ||
|#

#### Template Not Found

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Record with ID `999999` was not found | Check the template ID with the [tasks.template.list](./tasks-template-list-rest-v3.md) method. The method does not find a deleted template either ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permission to view the template with the [tasks.template.access.get](./tasks-template-access-get-rest-v3.md) method. With `withSubTasks: true`, permission to view each child template is required. Also make sure the new task card is available on Bitrix24 ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-task-get-rest-v3.md)
- [{#T}](./tasks-template-get-rest-v3.md)
- [{#T}](../tasks-task-get-rest-v3.md)
- [{#T}](../fields-rest-v3.md)
