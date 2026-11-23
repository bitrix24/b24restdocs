# Get Template Related Task List v 3.0 tasks.template.relation.related.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`tasks`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to read the template

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The method `tasks.template.relation.related.list` returns the tasks related to a template with pagination.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../../data-types.md) | Template identifier.

The template identifier can be obtained when [creating a template](../tasks-template-add-rest-v3.md) or by using the [template list](../tasks-template-list-rest-v3.md) method ||
|| **select**
[`string[]`](../../../data-types.md) | Task fields to return. If `select` is not passed, the method returns all fields from the list below.

Possible values:

- `id` — task identifier
- `title` — task title
- `responsibleId` — assignee identifier
- `responsible` — assignee object
- `deadline` — deadline
- `status` — task status
- `groupId` — group or project identifier
- `group` — group or project object
- `allowsChangeDeadline` — whether the assignee is allowed to change the deadline
- `activity` — date of last activity
- `changed` — change date

For fields of the `responsible` and `group` objects, use a dot path, for example `["title","responsible.name"]`. For a detailed description of the fields, see the [task object description](../../fields-rest-v3.md#taskdto) ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination parameters:

- `page` — page number
- `limit` — number of tasks per page, `50` by default, `1000` at most
- `offset` — offset from the beginning of the list. If `page` and `limit` are passed, the offset is calculated automatically ||
|#

{% note info "" %}

The method does not support the `filter` and `order` parameters: with them, the method returns an error. The `id` field is always returned, even if it is not in `select`.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.template.relation.related.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":117,"select":["title","status","deadline","responsible.name"],"pagination":{"page":1,"limit":20}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.relation.related.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"templateId":117,"select":["title","status","deadline","responsible.name"],"pagination":{"page":1,"limit":20},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.template.relation.related.list
    ```

- JS (TS)

    The `tasks.template.relation.*` methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`: such a call returns the SDK error `JSSDK_CORE_METHOD_NOT_SUPPORT_IN_API_V3`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(ApiVersion.v3).call`.

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { ApiVersion, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type RelatedListResult = {
      items: Array<{
        id: number
        title: string
        status: string
        deadline: ISODate | null
        responsible: {
          name: string
        }
      }>
    }

    try {
      const response = await $b24.getHttpClient(ApiVersion.v3).call<RelatedListResult>(
        'tasks.template.relation.related.list',
        {
          templateId: 117,
          select: ['title', 'status', 'deadline', 'responsible.name'],
          pagination: {
            page: 1,
            limit: 20,
          },
        },
        Text.getUuidRfc4122()
      )

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        for (const item of result.items) {
          console.info(item.id, item.title, item.status, item.responsible.name)
        }
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    The `tasks.template.relation.*` methods are not yet included in the list of REST 3.0 methods known to `$b24.actions.v3.call.make`. Call the method through the REST 3.0 HTTP client — `$b24.getHttpClient(B24Js.ApiVersion.v3).call`.

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getRelatedTasks() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.getHttpClient(B24Js.ApiVersion.v3).call(
            'tasks.template.relation.related.list',
            {
              templateId: 117,
              select: ['title', 'status', 'deadline', 'responsible.name'],
              pagination: {
                page: 1,
                limit: 20,
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
          result.items.forEach(item => console.info(item.id, item.title, item.status, item.responsible.name))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getRelatedTasks)
    </script>
    ```

- Python

    The `tasks.template.relation.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `bitrix_token.call_method`, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "tasks.template.relation.related.list"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "templateId": 117,
                "select": ["title", "status", "deadline", "responsible.name"],
                "pagination": {
                    "page": 1,
                    "limit": 20,
                },
            },
            prefer_version=3,
        )
        for item in response["result"]["items"]:
            print(item["id"], item["title"], item["status"], item["responsible"]["name"])
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
                'tasks.template.relation.related.list',
                [
                    'templateId' => 117,
                    'select' => ['title', 'status', 'deadline', 'responsible.name'],
                    'pagination' => [
                        'page' => 1,
                        'limit' => 20,
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
        'tasks.template.relation.related.list',
        {
            templateId: 117,
            select: ['title', 'status', 'deadline', 'responsible.name'],
            pagination: {
                page: 1,
                limit: 20,
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
        'tasks.template.relation.related.list',
        [
            'templateId' => 117,
            'select' => ['title', 'status', 'deadline', 'responsible.name'],
            'pagination' => [
                'page' => 1,
                'limit' => 20,
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
    res, err := client.Core().Call(ctx, "tasks.template.relation.related.list", b24.Params{
    	"templateId": 117,
    	"select":     []string{"title", "status", "deadline", "responsible.name"},
    	"pagination": map[string]int{"page": 1, "limit": 20},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.template.relation.related.list: %w", err)
    }

    var out struct {
    	Items []struct {
    		ID          int    `json:"id"`
    		Title       string `json:"title"`
    		Status      string `json:"status"`
    		Deadline    string `json:"deadline"`
    		Responsible struct {
    			Name string `json:"name"`
    		} `json:"responsible"`
    	} `json:"items"`
    }
    if err := json.Unmarshal(res.Result, &out); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, item := range out.Items {
    	fmt.Println(item.ID, item.Title, item.Status, item.Responsible.Name)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

Response to the request from the example:

```json
{
    "result": {
        "items": [
            {
                "id": 4401,
                "title": "Approve the onboarding plan",
                "responsible": {
                    "name": "Anna Schmidt"
                },
                "deadline": "2026-11-30T20:00:00+01:00",
                "status": "pending"
            },
            {
                "id": 4399,
                "title": "Prepare the employment order",
                "responsible": {
                    "name": "Anna Schmidt"
                },
                "deadline": "2026-11-30T20:00:00+01:00",
                "status": "pending"
            }
        ]
    },
    "time": {
        "start": 1795429118,
        "finish": 1795429118.215197,
        "duration": 0.21519708633422852,
        "processing": 0,
        "date_start": "2026-11-23T11:18:38+01:00",
        "date_finish": "2026-11-23T11:18:38+01:00",
        "operating_reset_at": 1795429680,
        "operating": 0
    }
}
```

A response without `select` contains all task fields:

```json
{
    "result": {
        "items": [
            {
                "id": 4401,
                "title": "Approve the onboarding plan",
                "responsibleId": 1269,
                "responsible": {
                    "id": 1269,
                    "name": "Anna Schmidt",
                    "role": null,
                    "image": {
                        "id": null,
                        "src": "https://cdn-com.bitrix24.com/b13743910/resize_cache/32741/45a41af90f4a8f8ae94152f60a6ec694/main/bd3/bd36722eacdbe1263e452943ce21a820/normal.jpg.png",
                        "name": null,
                        "width": 100,
                        "height": 100,
                        "size": 24131,
                        "subDir": null,
                        "contentType": null,
                        "file": null
                    },
                    "gender": "F",
                    "email": "",
                    "externalAuthId": null,
                    "rights": []
                },
                "deadline": "2026-11-30T20:00:00+01:00",
                "groupId": null,
                "group": null,
                "status": "pending",
                "changed": "2026-11-23T11:16:44+01:00",
                "activity": "2026-11-23T11:16:44+01:00",
                "allowsChangeDeadline": false
            }
        ]
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response. Contains the `items` array ||
|| **items**
[`object[]`](../../../data-types.md) | Array of related tasks of the template [(detailed description)](#items). If there are no related tasks, the array is empty ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Element of the items Array {#items}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Task identifier ||
|| **title**
[`string`](../../../data-types.md) | Task title ||
|| **responsibleId**
[`integer`](../../../data-types.md) | Assignee identifier ||
|| **responsible**
[`object`](../../fields-rest-v3.md#user) | Assignee. An object of type [user](../../fields-rest-v3.md#user) ||
|| **deadline**
[`datetime`](../../../data-types.md) | Deadline in ISO 8601 format. `null` if no deadline is set ||
|| **groupId**
[`integer`](../../../data-types.md) | Group or project identifier. `null` if the task is not in a group ||
|| **group**
[`object`](../../fields-rest-v3.md#group) | Group or project. An object of type [group](../../fields-rest-v3.md#group). `null` if the task is not in a group ||
|| **status**
[`string`](../../../data-types.md) | Task status. Possible values:

- `pending` — waiting for execution
- `in_progress` — in progress
- `supposedly_completed` — awaiting control
- `completed` — completed
- `deferred` — deferred
- `declined` — declined ||
|| **changed**
[`datetime`](../../../data-types.md) | Change date in ISO 8601 format ||
|| **activity**
[`datetime`](../../../data-types.md) | Date of last activity in ISO 8601 format ||
|| **allowsChangeDeadline**
[`boolean`](../../../data-types.md) | Whether the assignee is allowed to change the deadline:

- `true` — allowed
- `false` — not allowed ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_FIELDNOTSUPPORTEDEXCEPTION",
        "message": "This method does not support \"priority\"."
    }
}
```

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

HTTP status: **400**

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `templateId` | Required field `templateId` is missing | Add `templateId` to the request body ||
|| `templateId` | This request requires data type `int` in field `templateId` | Pass the identifier as a number, not a string ||
|#

#### Parameter Not Supported

HTTP status: **400**

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_FIELDNOTSUPPORTEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter` | This method does not support `filter` | Remove `filter` from the request ||
|| `order` | This method does not support `order` | Remove `order` from the request ||
|| `select` | This method does not support `priority` | Keep only the fields listed in the `select` parameter description in `select` ||
|#

#### Pagination Error

HTTP status: **400**

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `pagination` | Incorrect `pagination` parameter value: `{"limit":0}` | Pass `limit` greater than zero, `offset` not less than zero, and `page` as a number ||
|#

#### Template Not Found

HTTP status: **404**

Error Code: `BITRIX_TASKS_V2_INFRASTRUCTURE_REST_EXCEPTION_TEMPLATE_NOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `templateId` | Record with ID `999999` was not found | Check the identifier using the [tasks.template.get](../tasks-template-get-rest-v3.md) method. The method treats a deleted template as not found ||
|#

#### Access Error

HTTP status: **403**

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has permission to read the template from `templateId` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-relation-related-add-rest-v3.md)
- [{#T}](./tasks-template-relation-related-delete-rest-v3.md)
- [{#T}](./tasks-template-relation-related-list-by-ids-rest-v3.md)
- [{#T}](../../fields-rest-v3.md)
