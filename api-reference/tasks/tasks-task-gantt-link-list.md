# Get Task Gantt Link List tasks.task.gantt.link.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`tasks`](../scopes/permissions.md)
>
> Who can execute the method: any user with access to the task

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../rest-v3.md).

{% endnote %}

The `tasks.task.gantt.link.list` method returns a list of outgoing Gantt links for a task. The response contains the linked task ID and the link type: Start-Start, Start-Finish, Finish-Start, or Finish-Finish.

The method does not create or delete links. To create links, use [task.dependence.add](./task-dependence-add.md). To delete links, use [task.dependence.delete](./task-dependence-delete.md).

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **filter***
[`array`](../data-types.md) | Filter for selecting Gantt links of a task.

Pass the required condition by the `taskId` field in the `["taskId", 101]` format.

Available filter field:
- `taskId` — task ID. The method checks read access to this task ||
|| **select**
[`array`](../data-types.md) | Array of fields to return.

If this parameter is not passed, the method returns all Gantt link fields.

Available fields:
- `taskId` — task ID
- `dependentId` — ID of the task linked to the task in `taskId`
- `type` — Gantt link type
- `creatorId` — ID of the user who created the link ||
|| **pagination**
[`object`](../data-types.md) | Object for managing pagination.

Pagination parameters:
- `page` — page number. If the value is less than one, the first page is used
- `limit` — number of items per page. Default value: `50`, maximum value: `1000`
- `offset` — offset from the beginning of the list. Default value: `0`

If `page` and `offset` are passed, the offset calculated by `page` is used.

The list is sorted by `dependentId` in ascending order. The `order` parameter is not supported ||
|#

### Gantt Link Types {#link-types}

#|
|| **Value** | **Description** ||
|| `start_start` | Start-Start: task start depends on linked task start ||
|| `start_finish` | Start-Finish: task start depends on linked task finish ||
|| `finish_start` | Finish-Start: task finish depends on linked task start ||
|| `finish_finish` | Finish-Finish: task finish depends on linked task finish ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.task.gantt.link.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":["taskId",101],"select":["taskId","dependentId","type","creatorId"],"pagination":{"limit":10,"offset":0}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.gantt.link.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":["taskId",101],"select":["taskId","dependentId","type","creatorId"],"pagination":{"limit":10,"offset":0},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.task.gantt.link.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type GanttLinkType = 'start_start' | 'start_finish' | 'finish_start' | 'finish_finish'

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type GanttLinkListResult = {
      items: {
        taskId: number
        dependentId: number
        type: GanttLinkType
        creatorId: number
      }[]
    }

    try {
      const response = await $b24.actions.v3.call.make<GanttLinkListResult>({
        method: 'tasks.task.gantt.link.list',
        params: {
          filter: ['taskId', 101],
          select: ['taskId', 'dependentId', 'type', 'creatorId'],
          pagination: {
            limit: 10,
            offset: 0,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.items)
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
      async function fetchGanttLinks() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'tasks.task.gantt.link.list',
            params: {
              filter: ['taskId', 101],
              select: ['taskId', 'dependentId', 'type', 'creatorId'],
              pagination: {
                limit: 10,
                offset: 0,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchGanttLinks)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    filter = [
        "taskId",
        101,
    ]

    select = [
        "taskId",
        "dependentId",
        "type",
        "creatorId",
    ]

    pagination = {
        "limit": 10,
        "offset": 0,
    }

    try:
        bitrix_response = client.tasks.task.gantt.link.list(
            filter=filter,
            select=select,
            pagination=pagination,
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

    SDKs do not yet support the /rest/api/ path in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.gantt.link.list',
                [
                    'filter' => ['taskId', 101],
                    'select' => ['taskId', 'dependentId', 'type', 'creatorId'],
                    'pagination' => [
                        'limit' => 10,
                        'offset' => 0,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
    }
    ```

- BX24.js

    SDKs do not yet support the /rest/api/ path in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```js
    BX24.callMethod(
        'tasks.task.gantt.link.list',
        {
            filter: ['taskId', 101],
            select: ['taskId', 'dependentId', 'type', 'creatorId'],
            pagination: {
                limit: 10,
                offset: 0
            }
        },
        function(result){
            console.info(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the /rest/api/ path in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.gantt.link.list',
        [
            'filter' => ['taskId', 101],
            'select' => ['taskId', 'dependentId', 'type', 'creatorId'],
            'pagination' => [
                'limit' => 10,
                'offset' => 0,
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx have already been created — see the "Go SDK" section
    res, err := client.Core().Call(ctx, "tasks.task.gantt.link.list", b24.Params{
    	"filter": []any{"taskId", 101},
    	"select": []string{"taskId", "dependentId", "type", "creatorId"},
    	"pagination": b24.Params{
    		"limit":  10,
    		"offset": 0,
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.task.gantt.link.list: %w", err)
    }

    // The method wraps the response in an object with the "items" key.
    raw, ok := b24.Unwrap(res.Result, "items")
    if !ok {
    	return fmt.Errorf("response does not contain the items key")
    }

    var items []struct {
    	TaskID      b24.ID `json:"taskId"`
    	DependentID b24.ID `json:"dependentId"`
    	Type        string `json:"type"`
    	CreatorID   b24.ID `json:"creatorId"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.DependentID, it.Type)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "taskId": 101,
                "dependentId": 205,
                "type": "finish_start",
                "creatorId": 1
            }
        ]
    },
    "time": {
        "start": 1787754442,
        "finish": 1787754442.612709,
        "duration": 0.6127090454101562,
        "processing": 0,
        "date_start": "2026-08-26T17:27:22+03:00",
        "date_finish": "2026-08-26T17:27:22+03:00",
        "operating_reset_at": 1787755042,
        "operating": 0
    }
}
```

If the task has no outgoing Gantt links, the method returns an empty `items` array.

```json
{
    "result": {
        "items": []
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Object with response data [(detailed description)](#result) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **items**
[`array`](../data-types.md) | Array of task Gantt links.

Item fields depend on the `select` parameter ||
|#

#### Gantt Link Object {#gantt-link}

#|
|| **Name**
`type` | **Description** ||
|| **taskId**
[`integer`](../data-types.md) | Task ID ||
|| **dependentId**
[`integer`](../data-types.md) | ID of the task linked to the task in `taskId` ||
|| **type**
[`string`](../data-types.md) | Link type. Possible values are described in the [Gantt Link Types](#link-types) table ||
|| **creatorId**
[`integer`](../data-types.md) | ID of the user who created the link ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Request object validation error",
        "validation": [
            {
                "field": "filter",
                "message": "Field value cannot be empty"
            }
        ]
    }
}
```

{% include notitle [error handling](../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter` | Field value cannot be empty | Pass the `["taskId", 101]` filter, where `101` is the task ID ||
|| `taskId` | The `taskId` field requires the `int` data type for this request | Pass `taskId` as a number greater than zero ||
|| `filter`
`order` | The field does not support filtering or sorting | Pass only `taskId` in `filter`. Do not pass the `order` parameter, the order is fixed ||
|#

Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter.taskId` | A filter by the required `taskId` field must be specified | Pass the `["taskId", 101]` filter ||
|#

Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter` | Cannot parse the filter expression | Pass the filter as an array, for example `["taskId", 101]`. The `{"taskId": 101}` object is not supported ||
|#

Error code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter`
`select`
`order` | Unknown field `#FIELD#` for the `GanttLinkDto` object | Pass only the `taskId`, `dependentId`, `type`, and `creatorId` fields. Only `taskId` is available in `filter` ||
|#

Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Cannot parse the select expression `#SELECT#` | Pass `select` as an array of strings, for example `["taskId","dependentId","type"]` ||
|#

Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `pagination` | Cannot parse the pagination parameter | Pass `page`, `limit`, or `offset` as numbers. `limit` must be greater than zero, `offset` must be at least zero ||
|#

#### Access Error

Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `taskId` | Access denied | Check the user's read access to the task ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-task-get-rest-v3.md)
- [{#T}](./tasks-task-list-rest-v3.md)
- [{#T}](./task-dependence-add.md)
- [{#T}](./task-dependence-delete.md)
