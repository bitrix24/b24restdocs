# Get a Task Draft from a Template v 3.0 tasks.template.task.get

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

The `tasks.template.task.get` method returns a task draft filled in from a template without creating a task.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../data-types.md) | Task template ID.

The ID can be retrieved with the [tasks.template.add](./tasks-template-add-rest-v3.md) method when creating a template or with the [tasks.template.list](./tasks-template-list-rest-v3.md) method ||
|| **select**
[`string[]`](../../data-types.md) | Task fields to return. If the parameter is not passed, the method returns all task fields without related objects.

For related objects, use a path with a dot, for example, `["responsible.id","responsible.name"]`: the response contains the `responsible` object with the requested fields. [Task field description](../fields-rest-v3.md#taskdto) ||
|#

The draft shows the values with which the [tasks.template.task.add](./tasks-template-task-add-rest-v3.md) method creates a task if no fields are overridden. The draft has no ID: the `id` field is always `null` and is returned in the response even if it is not in `select`.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.task.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":125,"select":["title","description","responsibleId","deadline","priority","crmItemIds"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.task.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":125,"select":["title","description","responsibleId","deadline","priority","crmItemIds"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.task.get
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
    type TemplateTaskGetResult = {
      item: {
        id: null
        title: string
        description: string
        responsibleId: number
        deadline: ISODate | null
        priority: string
        crmItemIds: string[]
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateTaskGetResult>(
        'tasks.template.task.get',
        {
          templateId: 125,
          select: ['title', 'description', 'responsibleId', 'deadline', 'priority', 'crmItemIds'],
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Task draft:', result.item.title, result.item.responsibleId)
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
      async function getTaskDraft() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.task.get',
            {
              templateId: 125,
              select: ['title', 'description', 'responsibleId', 'deadline', 'priority', 'crmItemIds'],
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Task draft:', result.item.title, result.item.responsibleId)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTaskDraft)
    </script>
    ```

- Python

    REST 3.0 task template methods are not yet included in the `Config().api_v3_methods` list, and without it `b24pysdk` sends the call to the old REST address. Add the method to this list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.task.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "templateId": 125,
                "select": ["title", "description", "responsibleId", "deadline", "priority", "crmItemIds"],
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
                'tasks.template.task.get',
                [
                    'templateId' => 125,
                    'select' => ['title', 'description', 'responsibleId', 'deadline', 'priority', 'crmItemIds'],
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
        'tasks.template.task.get',
        {
            templateId: 125,
            select: ['title', 'description', 'responsibleId', 'deadline', 'priority', 'crmItemIds'],
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
        'tasks.template.task.get',
        [
            'templateId' => 125,
            'select' => ['title', 'description', 'responsibleId', 'deadline', 'priority', 'crmItemIds'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "tasks.template.task.get", b24.Params{
    	"templateId": 125,
    	"select":     []string{"title", "description", "responsibleId", "deadline", "priority", "crmItemIds"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.template.task.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var draft struct {
    	Title         string   `json:"title"`
    	Description   string   `json:"description"`
    	ResponsibleID int      `json:"responsibleId"`
    	Deadline      *string  `json:"deadline"`
    	Priority      string   `json:"priority"`
    	CrmItemIDs    []string `json:"crmItemIds"`
    }
    if err := json.Unmarshal(raw, &draft); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(draft.Title, draft.ResponsibleID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

Response to the request from the examples:

```json
{
    "result": {
        "item": {
            "id": null,
            "title": "Weekly sales report",
            "description": "Prepare a report on the week's results",
            "responsibleId": 1269,
            "deadline": null,
            "priority": "average",
            "crmItemIds": [
                "D_9193",
                "C_4575",
                "CO_2985",
                "L_1001345",
                "SI_117"
            ]
        }
    },
    "time": {
        "start": 1795429068,
        "finish": 1795429068.387838,
        "duration": 0.38783788681030273,
        "processing": 0,
        "date_start": "2026-11-23T11:17:48+01:00",
        "date_finish": "2026-11-23T11:17:48+01:00",
        "operating_reset_at": 1795429620,
        "operating": 0
    }
}
```

Without `select`, the method returns all task fields. Fields that the template does not fill in are `null`:

```json
{
    "result": {
        "item": {
            "id": null,
            "title": "Weekly sales report",
            "description": "Prepare a report on the week's results",
            "creatorId": 1269,
            "responsibleId": 1269,
            "deadline": null,
            "needsControl": false,
            "startPlan": null,
            "endPlan": null,
            "fileIds": null,
            "checklist": [],
            "groupId": 0,
            "stageId": 0,
            "epicId": null,
            "storyPoints": null,
            "priority": "average",
            "status": null,
            "statusChanged": null,
            "parentId": null,
            "containsChecklist": false,
            "containsSubTasks": false,
            "containsRelatedTasks": false,
            "containsGanttLinks": null,
            "containsPlacements": null,
            "containsResults": null,
            "numberOfReminders": null,
            "chatId": null,
            "plannedDuration": null,
            "actualDuration": null,
            "durationType": null,
            "started": null,
            "estimatedTime": 0,
            "replicate": false,
            "changed": null,
            "closed": null,
            "activity": null,
            "guid": null,
            "xmlId": null,
            "exchangeId": null,
            "exchangeModified": null,
            "outlookVersion": null,
            "mark": null,
            "allowsChangeDeadline": false,
            "allowsTimeTracking": false,
            "matchesWorkTime": false,
            "addInReport": false,
            "isMultitask": false,
            "siteId": "s1",
            "deadlineCount": null,
            "declineReason": null,
            "forumTopicId": null,
            "link": "/company/personal/user/1269/tasks/templates/template/view/125/",
            "rights": [],
            "archiveLink": "/bitrix/tools/disk/uf.php?entityId=125&entity=TASKS_TASK_TEMPLATE&fieldName=UF_TASK_WEBDAV_FILES&signature=cb2d54fcfcaf585d068040130da2ee67c2c48c0b2173b9c41884237a24a851c1&action=downloadArchiveByEntity&ncc=1",
            "crmItemIds": [
                "D_9193",
                "C_4575",
                "CO_2985",
                "L_1001345",
                "SI_117"
            ],
            "requireResult": null,
            "checklistAutonumbering": null,
            "matchesSubTasksTime": null,
            "autocompleteSubTasks": null,
            "allowsChangeDatePlan": false,
            "maxDeadlineChangeDate": null,
            "maxDeadlineChanges": null,
            "requireDeadlineChangeReason": null,
            "inFavorite": [],
            "inPin": [],
            "inGroupPin": [],
            "inMute": [],
            "dependsOn": [],
            "scenarios": []
        }
    },
    "time": {
        "start": 1795429067,
        "finish": 1795429067.647728,
        "duration": 0.6477279663085938,
        "processing": 0,
        "date_start": "2026-11-23T11:17:47+01:00",
        "date_finish": "2026-11-23T11:17:47+01:00",
        "operating_reset_at": 1795429620,
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
[`object`](../../data-types.md) | Task draft in the [task object](../fields-rest-v3.md#taskdto) format [(detailed description)](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object item {#item}

The draft fields match the [REST 3.0 task fields](../fields-rest-v3.md#taskdto). The draft differs from a created task:

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Always `null`: the task has not been created ||
|| **status**
[`string`](../../data-types.md) | Always `null`: the task receives a status on creation ||
|| **link**
[`string`](../../data-types.md) | Relative link to the template in the Bitrix24 interface, not to a task ||
|| **archiveLink**
[`string`](../../data-types.md) | Link to the archive of template files ||
|| **rights**
[`array`](../../data-types.md) | Empty array: permissions for the task appear after creation ||
|| **crmItemIds**
[`string[]`](../../data-types.md) | CRM item codes from the template, for example, `D_9193` — deal, `SI_117` — invoice ||
|#

## Error Handling

HTTP status: **400**, **403**, **404**

```json
{
    "error": {
        "code": "BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION",
        "message": "Record with ID \"999999\" was not found."
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
|#

#### Unknown Field in select

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `foo` found in entity `TaskDto` | Check the field names against the [task field description](../fields-rest-v3.md#taskdto) ||
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
|| `-` | Access denied | Check the user's permission to view the template with the [tasks.template.access.get](./tasks-template-access-get-rest-v3.md) method and make sure the new task card is available on Bitrix24 ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-task-add-rest-v3.md)
- [{#T}](./tasks-template-get-rest-v3.md)
- [{#T}](../fields-rest-v3.md)
- [{#T}](./fields-rest-v3.md)
