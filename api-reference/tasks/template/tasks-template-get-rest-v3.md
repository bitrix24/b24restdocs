# Get a Task Template v 3.0 tasks.template.get

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

The `tasks.template.get` method returns a task template by its ID.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Template ID.

The ID can be retrieved with the [tasks.template.list](./tasks-template-list-rest-v3.md) method or when [creating a template](./tasks-template-add-rest-v3.md) ||
|| **select**
[`array`](../../data-types.md) | List of fields to return. Field names match the keys of the [template object](./fields-rest-v3.md#templatedto).

Without `select`, the method returns simple template fields without nested and related objects.

The nested objects `tags`, `permissions`, `replicateParams`, and `userFields` are returned only if they are specified in `select`. You can specify the whole object, for example, `replicateParams`, or one of its fields with a dot, for example, `replicateParams.period`.

Related objects are returned if their fields are specified with a dot:
- `creator` — creator, for example, `creator.name`. [User fields](../fields-rest-v3.md#user)
- `group` — project, for example, `group.name`. [Group fields](../fields-rest-v3.md#group)
- `task` — task from which the template was created. Only `task.id` and `task.title` are available

The method does not return the `parent`, `templateChildrenCount`, and `subTemplateIds` fields ||
|#

{% note warning "" %}

If the template has no project, `groupId` equals `0`, and a request with `group` fields in `select` fails with the `INTERNAL_SERVER_ERROR` error. Request `group` only for templates whose `groupId` is greater than `0`.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":143,"select":["id","title","description","creatorId","creator.name","responsibleCollection","auditors","priority","deadlineAfterTs","replicate","replicateParams","tags","permissions"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":143,"select":["id","title","description","creatorId","creator.name","responsibleCollection","auditors","priority","deadlineAfterTs","replicate","replicateParams","tags","permissions"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.get
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
    type TemplateGetResult = {
      item: {
        id: number
        title: string
        description?: string
        creatorId: number
        creator?: { name: string }
        responsibleCollection?: Array<{ id: number }>
        auditors?: Array<{ id: number }>
        priority: 'low' | 'average' | 'high'
        deadlineAfterTs?: number
        replicate: boolean
        replicateParams?: Record<string, string | number | number[]>
        tags?: Array<{ id: number, name: string, ownerId: number }>
        permissions?: Array<{ id: number, accessCode: string, permission: 'read_only' | 'full' }>
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateGetResult>(
        'tasks.template.get',
        {
          id: 143,
          select: [
            'id', 'title', 'description', 'creatorId', 'creator.name',
            'responsibleCollection', 'auditors', 'priority', 'deadlineAfterTs',
            'replicate', 'replicateParams', 'tags', 'permissions',
          ],
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Template:', result.item.id, result.item.title)
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
      async function getTemplate() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.get',
            {
              id: 143,
              select: [
                'id', 'title', 'description', 'creatorId', 'creator.name',
                'responsibleCollection', 'auditors', 'priority', 'deadlineAfterTs',
                'replicate', 'replicateParams', 'tags', 'permissions',
              ],
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Template:', result.item.id, result.item.title)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTemplate)
    </script>
    ```

- Python

    The method name matches the previous version method `tasks.template.get`, and REST 3.0 template methods are not yet included in the list of new version methods in `b24pysdk`: a regular SDK call goes to the previous REST version. Add the method to the `Config().api_v3_methods` list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 143,
                "select": [
                    "id", "title", "description", "creatorId", "creator.name",
                    "responsibleCollection", "auditors", "priority", "deadlineAfterTs",
                    "replicate", "replicateParams", "tags", "permissions",
                ],
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result["item"])
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
                'tasks.template.get',
                [
                    'id' => 143,
                    'select' => [
                        'id', 'title', 'description', 'creatorId', 'creator.name',
                        'responsibleCollection', 'auditors', 'priority', 'deadlineAfterTs',
                        'replicate', 'replicateParams', 'tags', 'permissions',
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
        'tasks.template.get',
        {
            id: 143,
            select: [
                'id', 'title', 'description', 'creatorId', 'creator.name',
                'responsibleCollection', 'auditors', 'priority', 'deadlineAfterTs',
                'replicate', 'replicateParams', 'tags', 'permissions',
            ],
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
        'tasks.template.get',
        [
            'id' => 143,
            'select' => [
                'id', 'title', 'description', 'creatorId', 'creator.name',
                'responsibleCollection', 'auditors', 'priority', 'deadlineAfterTs',
                'replicate', 'replicateParams', 'tags', 'permissions',
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
    res, err := client.Core().Call(ctx, "tasks.template.get", b24.Params{
    	"id": 143,
    	"select": []string{
    		"id", "title", "description", "creatorId", "creator.name",
    		"responsibleCollection", "auditors", "priority", "deadlineAfterTs",
    		"replicate", "replicateParams", "tags", "permissions",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.template.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var template struct {
    	ID        int    `json:"id"`
    	Title     string `json:"title"`
    	Priority  string `json:"priority"`
    	Replicate bool   `json:"replicate"`
    }
    if err := json.Unmarshal(raw, &template); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(template.ID, template.Title, template.Replicate)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 143,
            "title": "Monthly sales report",
            "description": "Collect sales data for the week",
            "creatorId": 1269,
            "creator": {
                "name": "Anna Schmidt"
            },
            "responsibleCollection": [
                {
                    "id": 1269
                }
            ],
            "auditors": [
                {
                    "id": 29
                }
            ],
            "priority": "high",
            "deadlineAfterTs": 86400,
            "replicate": true,
            "replicateParams": {
                "period": "monthly",
                "everyDay": "1",
                "workdayOnly": "N",
                "everyWeek": "1",
                "monthlyType": "1",
                "monthlyDayNum": "25",
                "monthlyMonthNum1": "1",
                "monthlyMonthNum2": "1",
                "yearlyType": "1",
                "yearlyDayNum": "1",
                "time": "09:00",
                "timezoneOffset": "0",
                "repeatTill": "times",
                "times": "6",
                "weekDays": [],
                "deadlineOffset": 86400,
                "nextExecutionTime": "11/25/2026 09:00:00 am"
            },
            "tags": [
                {
                    "id": 25,
                    "name": "report",
                    "ownerId": 1269
                }
            ],
            "permissions": [
                {
                    "id": 299,
                    "accessCode": "U29",
                    "permission": "read_only"
                }
            ]
        }
    },
    "time": {
        "start": 1795429349,
        "finish": 1795429349.223943,
        "duration": 0.22394299507141113,
        "processing": 0,
        "date_start": "2026-11-23T11:22:29+01:00",
        "date_finish": "2026-11-23T11:22:29+01:00",
        "operating_reset_at": 1795429380,
        "operating": 0.10017204284667969
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
[`object`](./fields-rest-v3.md#templatedto) | Object with template fields. The set of fields depends on `select`. Fields with the `null` value are not included in the response. [Template field description](./fields-rest-v3.md#templatedto) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **404**

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
|| `id` | Required field `id` is missing | Pass the template ID ||
|| `id` | This request requires data type `integer` in field `id` | Pass a positive integer ||
|#

#### Errors in the select Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `foo` found in entity `TemplateDto` | Specify fields from the [Template Object](./fields-rest-v3.md#templatedto) table ||
|| `select` | Unknown field `foo` found in entity `UserDto` | Specify fields of the [user object](../fields-rest-v3.md#user) ||
|#

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_FIELDNOTSUPPORTEDEXCEPTION`

HTTP status: **400**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | This method does not support `subTemplateIds` | Remove the `parent`, `templateChildrenCount`, and `subTemplateIds` fields from `select`. Child templates are returned by the [tasks.template.list](./tasks-template-list-rest-v3.md) method ||
|| `select` | This method does not support `task.description` | For the `task` object, specify only `task.id` or `task.title` ||
|| `select` | This method does not support `tags.taskId` | For the `tags` object, specify `tags.id`, `tags.name`, or `tags.ownerId` ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has permission to view the template. Template permissions are returned by the [tasks.template.access.get](./tasks-template-access-get-rest-v3.md) method ||
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

- [{#T}](./tasks-template-list-rest-v3.md)
- [{#T}](./tasks-template-update-rest-v3.md)
- [{#T}](./tasks-template-copy-rest-v3.md)
- [{#T}](./fields-rest-v3.md)
- [{#T}](./tasks-template-access-get-rest-v3.md)
- [{#T}](./checklist/tasks-template-checklist-get-rest-v3.md)
- [{#T}](./tasks-template-get.md)
