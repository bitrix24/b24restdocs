# Add a Task Template v 3.0 tasks.template.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `tasks.template.add` method creates a task template and returns it as saved.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Template fields [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

The table lists the main fields. The full list of editable fields is described in the [Template Object](./fields-rest-v3.md#templatedto) table.

#|
|| **Name**
`type` | **Description** ||
|| **title***
[`string`](../../data-types.md) | Title of the template and of the task created from it ||
|| **creatorId***
[`integer`](../../data-types.md) | Creator ID. A user without administrator permissions specifies their own ID; only an administrator can assign another creator.

The user ID can be retrieved with the [user.get](../../user/user-get.md) method ||
|| **responsibleId***
[`integer`](../../data-types.md) | Responsible person ID. Required if `responsibleCollection` is not passed ||
|| **responsibleCollection**
[`array`](../../data-types.md) | List of responsible persons, for example, `[{"id": 1269}, {"id": 29}]`. If it is passed, `responsibleId` is ignored ||
|| **description**
[`string`](../../data-types.md) | Description in BB code format ||
|| **accomplices**
[`array`](../../data-types.md) | Participants, for example, `[{"id": 29}]` ||
|| **auditors**
[`array`](../../data-types.md) | Auditors, for example, `[{"id": 1}]` ||
|| **groupId**
[`integer`](../../data-types.md) | Project ID. Can be retrieved with the [sonet_group.get](../../sonet-group/sonet-group-get.md) method ||
|| **priority**
[`string`](../../data-types.md) | Priority. Possible values:
- `low` — low
- `average` — medium
- `high` — high

Default: `average` ||
|| **tParamType**
[`string`](../../data-types.md) | Template type. Possible values:
- `usual` — regular template
- `new_users` — template for a new employee

Default: `usual`. The type cannot be changed after creation ||
|| **deadlineAfterTs**
[`integer`](../../data-types.md) | Number of seconds after the task is created when its deadline occurs, for example, `86400` — one day later ||
|| **tags**
[`array`](../../data-types.md) | Tags, for example, `[{"name": "report"}]`. Only the `name` field is taken into account ||
|| **dependsOn**
[`array`](../../data-types.md) | IDs of the tasks on which the created task depends. No more than 50; the user must have read access to each task ||
|| **replicate**
[`boolean`](../../data-types.md) | Create tasks on a schedule. Possible values:
- `true` — recur
- `false` — do not recur

Default: `false`. For `true`, pass `replicateParams` ||
|| **replicateParams**
[`object`](./fields-rest-v3.md#replicate-params) | Recurrence schedule. Required parameters: `period` and `time`. [Recurrence parameters description](./fields-rest-v3.md#replicate-params) ||
|| **permissions**
[`array`](./fields-rest-v3.md#permissions) | Template access permissions, for example, `[{"accessCode": "U29", "permission": "read_only"}]`. The creator receives the `full` permission automatically. [Access permissions description](./fields-rest-v3.md#permissions) ||
|#

{% note warning "" %}

The method does not reject `priority` and `tParamType` values outside the list but saves them as `average` and `usual`. Check the values before the call.

{% endnote %}

Fields marked as read-only in the [template object](./fields-rest-v3.md#templatedto), for example, `id`, `status`, `stageId`, `xmlId`, `addInReport`, `checklist`, cannot be passed: the method returns the "Field is read-only" error. Add checklist items with the [tasks.template.checklist.*](./checklist/tasks-template-checklist-get-rest-v3.md) methods.

For a recurring template, the creator must have permission to assign tasks to the responsible persons and participants and to create tasks in the project from `groupId`.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"title":"Weekly sales report","description":"Collect sales data for the week","creatorId":1269,"responsibleId":1269,"auditors":[{"id":1}],"priority":"high","deadlineAfterTs":86400,"tags":[{"name":"report"}],"replicate":true,"replicateParams":{"period":"weekly","everyWeek":"1","weekDays":[5],"time":"10:00","repeatTill":"endless"}}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"title":"Weekly sales report","description":"Collect sales data for the week","creatorId":1269,"responsibleId":1269,"auditors":[{"id":1}],"priority":"high","deadlineAfterTs":86400,"tags":[{"name":"report"}],"replicate":true,"replicateParams":{"period":"weekly","everyWeek":"1","weekDays":[5],"time":"10:00","repeatTill":"endless"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.add
    ```

- JS (TS)

    REST 3.0 task template methods are not yet included in the list of methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TemplateAddResult = {
      item: {
        id: number
        title: string
        creatorId: number
        responsibleId: number
        priority: 'low' | 'average' | 'high'
        replicate: boolean
        replicateParams?: Record<string, string | number | number[]>
        tags: Array<{ id: number, name: string, ownerId: number }>
        permissions: Array<{ id: number, accessCode: string, permission: 'read_only' | 'full' }>
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateAddResult>(
        'tasks.template.add',
        {
          fields: {
            title: 'Weekly sales report',
            description: 'Collect sales data for the week',
            creatorId: 1269,
            responsibleId: 1269,
            auditors: [{ id: 1 }],
            priority: 'high',
            deadlineAfterTs: 86400,
            tags: [{ name: 'report' }],
            replicate: true,
            replicateParams: {
              period: 'weekly',
              everyWeek: '1',
              weekDays: [5],
              time: '10:00',
              repeatTill: 'endless',
            },
          },
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created template:', result.item.id)
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
      async function addTemplate() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.add',
            {
              fields: {
                title: 'Weekly sales report',
                description: 'Collect sales data for the week',
                creatorId: 1269,
                responsibleId: 1269,
                auditors: [{ id: 1 }],
                priority: 'high',
                deadlineAfterTs: 86400,
                tags: [{ name: 'report' }],
                replicate: true,
                replicateParams: {
                  period: 'weekly',
                  everyWeek: '1',
                  weekDays: [5],
                  time: '10:00',
                  repeatTill: 'endless',
                },
              },
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Created template:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addTemplate)
    </script>
    ```

- Python

    The method name matches the previous version method `tasks.template.add`, and REST 3.0 template methods are not yet included in the list of new version methods in `b24pysdk`: a regular SDK call goes to the previous REST version. Add the method to the `Config().api_v3_methods` list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "fields": {
                    "title": "Weekly sales report",
                    "description": "Collect sales data for the week",
                    "creatorId": 1269,
                    "responsibleId": 1269,
                    "auditors": [{"id": 1}],
                    "priority": "high",
                    "deadlineAfterTs": 86400,
                    "tags": [{"name": "report"}],
                    "replicate": True,
                    "replicateParams": {
                        "period": "weekly",
                        "everyWeek": "1",
                        "weekDays": [5],
                        "time": "10:00",
                        "repeatTill": "endless",
                    },
                },
            },
            prefer_version=3,
        )
        print(response["result"]["item"]["id"])
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
                'tasks.template.add',
                [
                    'fields' => [
                        'title' => 'Weekly sales report',
                        'description' => 'Collect sales data for the week',
                        'creatorId' => 1269,
                        'responsibleId' => 1269,
                        'auditors' => [['id' => 1]],
                        'priority' => 'high',
                        'deadlineAfterTs' => 86400,
                        'tags' => [['name' => 'report']],
                        'replicate' => true,
                        'replicateParams' => [
                            'period' => 'weekly',
                            'everyWeek' => '1',
                            'weekDays' => [5],
                            'time' => '10:00',
                            'repeatTill' => 'endless',
                        ],
                    ],
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
        'tasks.template.add',
        {
            fields: {
                title: 'Weekly sales report',
                description: 'Collect sales data for the week',
                creatorId: 1269,
                responsibleId: 1269,
                auditors: [{ id: 1 }],
                priority: 'high',
                deadlineAfterTs: 86400,
                tags: [{ name: 'report' }],
                replicate: true,
                replicateParams: {
                    period: 'weekly',
                    everyWeek: '1',
                    weekDays: [5],
                    time: '10:00',
                    repeatTill: 'endless',
                },
            },
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
        'tasks.template.add',
        [
            'fields' => [
                'title' => 'Weekly sales report',
                'description' => 'Collect sales data for the week',
                'creatorId' => 1269,
                'responsibleId' => 1269,
                'auditors' => [['id' => 1]],
                'priority' => 'high',
                'deadlineAfterTs' => 86400,
                'tags' => [['name' => 'report']],
                'replicate' => true,
                'replicateParams' => [
                    'period' => 'weekly',
                    'everyWeek' => '1',
                    'weekDays' => [5],
                    'time' => '10:00',
                    'repeatTill' => 'endless',
                ],
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "tasks.template.add", b24.Params{
    	"fields": map[string]any{
    		"title":           "Weekly sales report",
    		"description":     "Collect sales data for the week",
    		"creatorId":       1269,
    		"responsibleId":   1269,
    		"auditors":        []map[string]int{{"id": 1}},
    		"priority":        "high",
    		"deadlineAfterTs": 86400,
    		"tags":            []map[string]string{{"name": "report"}},
    		"replicate":       true,
    		"replicateParams": map[string]any{
    			"period":     "weekly",
    			"everyWeek":  "1",
    			"weekDays":   []int{5},
    			"time":       "10:00",
    			"repeatTill": "endless",
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("tasks.template.add: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var template struct {
    	ID    int    `json:"id"`
    	Title string `json:"title"`
    }
    if err := json.Unmarshal(raw, &template); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(template.ID, template.Title)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 143,
            "title": "Weekly sales report",
            "description": "Collect sales data for the week",
            "descriptionInBbcode": true,
            "creatorId": 1269,
            "responsibleId": 1269,
            "responsibleCollection": [
                {
                    "id": 1269
                }
            ],
            "responsibleLastName": "Schmidt",
            "accomplices": [],
            "auditors": [
                {
                    "id": 1
                }
            ],
            "groupId": 0,
            "taskId": 0,
            "stageId": 0,
            "xmlId": "",
            "status": "1",
            "addInReport": false,
            "priority": "high",
            "tParamType": "usual",
            "deadlineAfterTs": 86400,
            "startDatePlanTs": 0,
            "endDatePlanTs": 0,
            "allowsChangeDeadline": false,
            "allowsTimeTracking": false,
            "matchesWorkTime": false,
            "needsControl": false,
            "estimatedTime": 0,
            "multitask": false,
            "siteId": "s1",
            "replicate": true,
            "replicateParams": {
                "period": "weekly",
                "everyDay": "1",
                "workdayOnly": "N",
                "everyWeek": "1",
                "monthlyType": "1",
                "monthlyDayNum": "1",
                "monthlyMonthNum1": "1",
                "monthlyMonthNum2": "1",
                "yearlyType": "1",
                "yearlyDayNum": "1",
                "time": "10:00",
                "timezoneOffset": "0",
                "repeatTill": "endless",
                "weekDays": [
                    5
                ],
                "deadlineOffset": 86400,
                "nextExecutionTime": "11/27/2026 10:00:00 am"
            },
            "tParamReplicationCount": 0,
            "dependsOn": [],
            "crmItemIds": [],
            "checklist": [],
            "tags": [
                {
                    "id": 25,
                    "name": "report",
                    "ownerId": 1269
                }
            ],
            "permissions": [
                {
                    "id": 291,
                    "accessCode": "U1269",
                    "permission": "full"
                }
            ]
        }
    },
    "time": {
        "start": 1795429241,
        "finish": 1795429241.855626,
        "duration": 0.855626106262207,
        "processing": 0,
        "date_start": "2026-11-23T11:20:41+01:00",
        "date_finish": "2026-11-23T11:20:41+01:00",
        "operating_reset_at": 1795429800,
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
[`object`](./fields-rest-v3.md#templatedto) | The created template with all filled fields, including `tags`, `permissions`, and `replicateParams`. Fields with the `null` value are not included in the response. [Template field description](./fields-rest-v3.md#templatedto) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION",
        "message": "Error validating object.",
        "validation": [
            {
                "message": "Required field \"title\" is empty.",
                "field": "title"
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
|| `fields` | Required field `fields` is missing | Pass the template fields in the `fields` object ||
|| `replicate` | This request requires data type `bool` in field `replicate` | Pass `true` or `false`, not `Y` or `N` ||
|| `permissions.0.permission` | This request requires data type `PermissionType` in field `permissions.0.permission` | Pass `read_only` or `full` ||
|| `permissions.0.accessCode` | This request requires data type `string` in field `permissions.0.accessCode` | Pass the access code as a string, for example, `U29` ||
|| `replicateParams.monthlyType` | This request requires data type `MonthlyType` in field `replicateParams.monthlyType` | Pass `1` or `2`. The same rule applies to `yearlyType` ||
|| `dependsOn` | This request requires data type `integer` in field `dependsOn` | Pass an array of positive integers ||
|| `dependsOn` | This request requires data type `array` in field `dependsOn` | Pass no more than 50 task IDs ||
|#

#### Template Field Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `title` | Required field `title` is empty | Pass the template title ||
|| `creatorId` | Required field `creatorId` is empty | Pass the creator ID ||
|| `-` | Assignee not specified | Pass `responsibleId` or `responsibleCollection` ||
|| `status` | Field `status` is read-only | Remove the fields marked as read-only in the [template object](./fields-rest-v3.md#templatedto) from `fields` ||
|| `permissions.0.accessCode` | Required field `accessCode` is empty | Pass `accessCode` in each `permissions` element ||
|| `TASKS_TEMPLATE_REPLICATION_PARAMS_INVALID` | Could not save recurring task. Check the schedule | Pass `replicateParams` with the `period` and `time` parameters. Pass the `startDate` and `endDate` dates in the Bitrix24 date format ||
|#

#### Unknown Field

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Unknown field `foo` found in entity `TemplateDto` | Pass fields from the [Template Object](./fields-rest-v3.md#templatedto) table ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | No permission to create templates, no read access to a task from `dependsOn`, or, for a recurring template, the creator cannot assign tasks to the responsible person or participant ||
|#

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_AUTHORASSIGNMENTDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `creatorId` | Only your Bitrix24 administrator can assign another employee as the task owner of a task template | Specify the current user's ID in `creatorId` ||
|#

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_PERMISSIONSRESTRICTEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `permissions` | Template access permissions cannot be configured on your current plan. Remove the `permissions` field from the request | Remove `permissions` from `fields` ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-update-rest-v3.md)
- [{#T}](./tasks-template-get-rest-v3.md)
- [{#T}](./tasks-template-copy-rest-v3.md)
- [{#T}](./fields-rest-v3.md)
- [{#T}](./checklist/tasks-template-checklist-get-rest-v3.md)
- [{#T}](./tasks-template-task-add-rest-v3.md)
- [{#T}](./tasks-template-add.md)
