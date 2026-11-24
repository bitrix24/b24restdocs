# Update a Task Template v 3.0 tasks.template.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the template

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `tasks.template.update` method updates the passed fields of a task template.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Template ID.

The ID can be retrieved with the [tasks.template.list](./tasks-template-list-rest-v3.md) method or when [creating a template](./tasks-template-add-rest-v3.md) ||
|| **fields***
[`object`](../../data-types.md) | Template fields to change. Pass only the fields that change; the other fields stay the same.

Accepts the same fields as [tasks.template.add](./tasks-template-add-rest-v3.md#fields), except `creatorId` and `tParamType`. The full list of editable fields is in the [Template Object](./fields-rest-v3.md#templatedto) table ||
|#

How the method handles specific fields:
- `replicateParams` is replaced entirely. Parameters that are not passed receive default values, so pass the complete schedule with `period` and `time`
- `permissions` replaces the entire list of permissions. Permissions can be changed by an administrator, the template author, or a user with the `full` permission; otherwise, the method returns an access error
- `dependsOn` replaces the list of tasks visible to the user. Links to tasks the user has no access to are kept
- `accomplices`, `auditors`, `responsibleCollection`, and `tags` replace the lists entirely
- for a recurring template, the creator must have permission to assign tasks to the responsible persons and participants; otherwise, the method returns an access error

The current template data after the update can be retrieved with the [tasks.template.get](./tasks-template-get-rest-v3.md) method.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":143,"fields":{"title":"Monthly sales report","priority":"high","replicateParams":{"period":"monthly","monthlyType":"1","monthlyDayNum":"25","monthlyMonthNum1":"1","time":"09:00","repeatTill":"times","times":"6"},"permissions":[{"accessCode":"U29","permission":"read_only"}]}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":143,"fields":{"title":"Monthly sales report","priority":"high","replicateParams":{"period":"monthly","monthlyType":"1","monthlyDayNum":"25","monthlyMonthNum1":"1","time":"09:00","repeatTill":"times","times":"6"},"permissions":[{"accessCode":"U29","permission":"read_only"}]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.update
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
    type TemplateUpdateResult = {
      result: boolean
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateUpdateResult>(
        'tasks.template.update',
        {
          id: 143,
          fields: {
            title: 'Monthly sales report',
            priority: 'high',
            replicateParams: {
              period: 'monthly',
              monthlyType: '1',
              monthlyDayNum: '25',
              monthlyMonthNum1: '1',
              time: '09:00',
              repeatTill: 'times',
              times: '6',
            },
            permissions: [
              { accessCode: 'U29', permission: 'read_only' },
            ],
          },
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated:', result.result)
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
      async function updateTemplate() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.update',
            {
              id: 143,
              fields: {
                title: 'Monthly sales report',
                priority: 'high',
                replicateParams: {
                  period: 'monthly',
                  monthlyType: '1',
                  monthlyDayNum: '25',
                  monthlyMonthNum1: '1',
                  time: '09:00',
                  repeatTill: 'times',
                  times: '6',
                },
                permissions: [
                  { accessCode: 'U29', permission: 'read_only' },
                ],
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
          console.info('Updated:', result.result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateTemplate)
    </script>
    ```

- Python

    The method name matches the previous version method `tasks.template.update`, and REST 3.0 template methods are not yet included in the list of new version methods in `b24pysdk`: a regular SDK call goes to the previous REST version. Add the method to the `Config().api_v3_methods` list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.update"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 143,
                "fields": {
                    "title": "Monthly sales report",
                    "priority": "high",
                    "replicateParams": {
                        "period": "monthly",
                        "monthlyType": "1",
                        "monthlyDayNum": "25",
                        "monthlyMonthNum1": "1",
                        "time": "09:00",
                        "repeatTill": "times",
                        "times": "6",
                    },
                    "permissions": [
                        {"accessCode": "U29", "permission": "read_only"},
                    ],
                },
            },
            prefer_version=3,
        )
        print(response["result"]["result"])
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
                'tasks.template.update',
                [
                    'id' => 143,
                    'fields' => [
                        'title' => 'Monthly sales report',
                        'priority' => 'high',
                        'replicateParams' => [
                            'period' => 'monthly',
                            'monthlyType' => '1',
                            'monthlyDayNum' => '25',
                            'monthlyMonthNum1' => '1',
                            'time' => '09:00',
                            'repeatTill' => 'times',
                            'times' => '6',
                        ],
                        'permissions' => [
                            ['accessCode' => 'U29', 'permission' => 'read_only'],
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
        'tasks.template.update',
        {
            id: 143,
            fields: {
                title: 'Monthly sales report',
                priority: 'high',
                replicateParams: {
                    period: 'monthly',
                    monthlyType: '1',
                    monthlyDayNum: '25',
                    monthlyMonthNum1: '1',
                    time: '09:00',
                    repeatTill: 'times',
                    times: '6',
                },
                permissions: [
                    { accessCode: 'U29', permission: 'read_only' },
                ],
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
        'tasks.template.update',
        [
            'id' => 143,
            'fields' => [
                'title' => 'Monthly sales report',
                'priority' => 'high',
                'replicateParams' => [
                    'period' => 'monthly',
                    'monthlyType' => '1',
                    'monthlyDayNum' => '25',
                    'monthlyMonthNum1' => '1',
                    'time' => '09:00',
                    'repeatTill' => 'times',
                    'times' => '6',
                ],
                'permissions' => [
                    ['accessCode' => 'U29', 'permission' => 'read_only'],
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
    res, err := client.Core().Call(ctx, "tasks.template.update", b24.Params{
    	"id": 143,
    	"fields": map[string]any{
    		"title":    "Monthly sales report",
    		"priority": "high",
    		"replicateParams": map[string]any{
    			"period":           "monthly",
    			"monthlyType":      "1",
    			"monthlyDayNum":    "25",
    			"monthlyMonthNum1": "1",
    			"time":             "09:00",
    			"repeatTill":       "times",
    			"times":            "6",
    		},
    		"permissions": []map[string]string{
    			{"accessCode": "U29", "permission": "read_only"},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("tasks.template.update: %w", err)
    }

    var result struct {
    	Result bool `json:"result"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "result": true
    },
    "time": {
        "start": 1795429313,
        "finish": 1795429313.825033,
        "duration": 0.8250329494476318,
        "processing": 0,
        "date_start": "2026-11-23T11:21:53+01:00",
        "date_finish": "2026-11-23T11:21:53+01:00",
        "operating_reset_at": 1795429860,
        "operating": 0.42789196968078613
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response.

Contains an object with the `result` key and the `true` value if the template was updated ||
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
                "message": "Field \"tParamType\" is read-only.",
                "field": "tParamType"
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
|| `-` | At least one of the required fields must be not empty: id, filter | Pass the template ID in `id` ||
|| `id` | Required field `id` is missing | Pass `id`. The method does not support updating by `filter` ||
|| `id` | This request requires data type `integer` in field `id` | Pass a positive integer ||
|| `fields` | Required field `fields` is missing | Pass the fields to change in the `fields` object ||
|| `permissions.0.permission` | This request requires data type `PermissionType` in field `permissions.0.permission` | Pass `read_only` or `full` ||
|| `replicateParams.monthlyType` | This request requires data type `MonthlyType` in field `replicateParams.monthlyType` | Pass `1` or `2`. The same rule applies to `yearlyType` ||
|| `dependsOn` | This request requires data type `array` in field `dependsOn` | Pass no more than 50 task IDs ||
|#

#### Template Field Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `tParamType` | Field `tParamType` is read-only | The template type is set only on creation ||
|| `creatorId` | Field `creatorId` is read-only | The creator is set only on creation ||
|| `id` | Field `id` is read-only | Pass the ID in the `id` parameter, not in `fields` ||
|| `TASKS_TEMPLATE_REPLICATION_PARAMS_INVALID` | Could not save recurring task. Check the schedule | Pass the entire `replicateParams` with the `period` and `time` parameters. Pass the `startDate` and `endDate` dates in the Bitrix24 date format ||
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
|| `-` | Access denied | Check the permission to edit the template with the [tasks.template.access.get](./tasks-template-access-get-rest-v3.md) method. The error also occurs if the user changes `permissions` without full access, specifies an inaccessible task in `dependsOn`, or the creator of a recurring template cannot assign tasks to the responsible person or participant ||
|#

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_PERMISSIONSRESTRICTEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `permissions` | Template access permissions cannot be configured on your current plan. Remove the `permissions` field from the request | Remove `permissions` from `fields` ||
|#

#### Template Not Found

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Record with ID `999999` was not found | The template does not exist or has been deleted. Check the ID with the [tasks.template.list](./tasks-template-list-rest-v3.md) method ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-get-rest-v3.md)
- [{#T}](./tasks-template-add-rest-v3.md)
- [{#T}](./tasks-template-delete-rest-v3.md)
- [{#T}](./fields-rest-v3.md)
- [{#T}](./tasks-template-access-get-rest-v3.md)
- [{#T}](./tasks-template-update.md)
