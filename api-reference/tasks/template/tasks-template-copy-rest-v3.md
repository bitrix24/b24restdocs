# Copy a Task Template v 3.0 tasks.template.copy

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to view the source template and to create templates

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `tasks.template.copy` method creates a copy of a task template and returns it as saved.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | ID of the source template.

The ID can be retrieved with the [tasks.template.list](./tasks-template-list-rest-v3.md) method ||
|#

What the copy receives:
- the title and the other fields of the source template without changes, including responsible persons, tags, access permissions, and recurrence parameters
- the user who created the copy receives the `full` permission in `permissions`
- the link to the source task is reset: `taskId` in the copy equals `0`
- if the source template is recurring, the copy with `replicate: true` also starts creating tasks on the schedule. To disable recurrence, pass `replicate: false` to [tasks.template.update](./tasks-template-update-rest-v3.md)

The source template does not change. The method supports the `Idempotency-Key` header: repeating the request with the same key returns the same copy instead of creating a second one. For more details, see [Repeated Calls Without Duplicates](../../rest-v3.md#idempotency).

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.copy`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":143}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.copy
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":143,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.copy
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
    type TemplateCopyResult = {
      item: {
        id: number
        title: string
        taskId: number
        replicate: boolean
      }
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<TemplateCopyResult>(
        'tasks.template.copy',
        {
          id: 143,
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Copy:', result.item.id, 'replicate:', result.item.replicate)
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
      async function copyTemplate() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.copy',
            {
              id: 143,
            },
            B24Js.Text.getUuidRfc4122()
          )

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Copy:', result.item.id, 'replicate:', result.item.replicate)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', copyTemplate)
    </script>
    ```

- Python

    REST 3.0 template methods are not yet included in the list of new version methods in `b24pysdk`, and the SDK has no ready-made wrapper for this method. Add the method to the `Config().api_v3_methods` list and call it via `bitrix_token.call_method` with `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.copy"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 143,
            },
            prefer_version=3,
        )
        item = response["result"]["item"]
        print(item["id"], item["replicate"])
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
                'tasks.template.copy',
                [
                    'id' => 143,
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
        'tasks.template.copy',
        {
            id: 143,
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
        'tasks.template.copy',
        [
            'id' => 143,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created; the client's base URL contains /rest/api/
    res, err := client.Core().Call(ctx, "tasks.template.copy", b24.Params{
    	"id": 143,
    })
    if err != nil {
    	return fmt.Errorf("tasks.template.copy: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var copied struct {
    	ID        int    `json:"id"`
    	Title     string `json:"title"`
    	Replicate bool   `json:"replicate"`
    }
    if err := json.Unmarshal(raw, &copied); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(copied.ID, copied.Title, copied.Replicate)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 149,
            "title": "Monthly sales report",
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
            "accomplices": [
                {
                    "id": 1269
                }
            ],
            "auditors": [
                {
                    "id": 29
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
            "tParamReplicationCount": 0,
            "dependsOn": [],
            "crmItemIds": [],
            "checklist": [],
            "tags": [
                {
                    "id": 27,
                    "name": "report",
                    "ownerId": 1269
                }
            ],
            "permissions": [
                {
                    "id": 303,
                    "accessCode": "U29",
                    "permission": "read_only"
                },
                {
                    "id": 305,
                    "accessCode": "U1269",
                    "permission": "full"
                }
            ]
        }
    },
    "time": {
        "start": 1795429316,
        "finish": 1795429316.719843,
        "duration": 0.7198429107666016,
        "processing": 0,
        "date_start": "2026-11-23T11:21:56+01:00",
        "date_finish": "2026-11-23T11:21:56+01:00",
        "operating_reset_at": 1795429860,
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
[`object`](./fields-rest-v3.md#templatedto) | Copy of the template with all filled fields, including `tags`, `permissions`, and `replicateParams`. Fields with the `null` value are not included in the response. [Template field description](./fields-rest-v3.md#templatedto) ||
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
|| `id` | Required field `id` is missing | Pass the ID of the source template ||
|| `id` | This request requires data type `int` in field `id` | Pass an integer ||
|#

#### Access Denied

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has permission to view the source template and to create templates. Template permissions are returned by the [tasks.template.access.get](./tasks-template-access-get-rest-v3.md) method ||
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
- [{#T}](./tasks-template-update-rest-v3.md)
- [{#T}](./tasks-template-add-rest-v3.md)
- [{#T}](./tasks-template-delete-rest-v3.md)
- [{#T}](./fields-rest-v3.md)
- [{#T}](./tasks-template-task-add-rest-v3.md)
