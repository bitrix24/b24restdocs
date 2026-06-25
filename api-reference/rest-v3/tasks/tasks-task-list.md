# Get a List of Tasks tasks.task.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `tasks.task.list` method returns a list of tasks based on the specified conditions.

Access to the data depends on permissions:
- An administrator sees all tasks
- A manager sees the tasks of their employees
- Others see only the tasks available to them

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | List of fields to be returned in the response. If `select` is not specified, the method returns only the `id` of the task.

Field names for `select` match the keys of the task object from the [Task object](./fields.md#taskdto) block ||
|| **filter**
[`array`](../../data-types.md) | Task filtering conditions in the format:
- `["field", "operator", value]`
- `["field", value]`, default operator `=`

In REST 3.0, filtering by the `id` field is supported for tasks.

[More details about filtering in REST 3.0](../index.md#filter) ||
|| **order**
[`object`](../../data-types.md) | Sorting of results in the `{ "field": "value" }` format.

Available values:
- `ASC` — ascending
- `DESC` — descending

Available fields for sorting: `id`, `title`, `creatorId`, `created`, `responsibleId`, `deadline`, `startPlan`, `endPlan`, `groupId`, `priority`, `status`, `started`, `estimatedTime`, `changed`, `closed`, `activity`, `mark`, `allowsChangeDeadline`, `allowsTimeTracking`.

See the field descriptions in the [Task object](./fields.md#taskdto) block

By default, tasks are sorted by `id` in ascending order ||
|| **pagination**
[`object`](../../data-types.md) | Pagination parameters:
- `page` — page number
- `limit` — number of tasks per page, default `50`, maximum `1000`
- `offset` — task offset. If `page` and `limit` are provided, the offset is calculated automatically ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.task.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","title","responsibleId","deadline","status"],"filter":[["id",">",1000]],"order":{"id":"ASC"},"pagination":{"page":1,"limit":20,"offset":0}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","title","responsibleId","deadline","status"],"filter":[["id",">",1000]],"order":{"id":"ASC"},"pagination":{"page":1,"limit":20,"offset":0},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.task.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type TaskListItem = {
      id: number
      title: string
      responsibleId: number
      deadline: ISODate | null
      status: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TaskListResult = {
      items: TaskListItem[]
    }

    try {
      // tasks.task.list returns a single page. For the whole result set
      // use a list helper: $b24.actions.v3.callList.make() returns every record as one
      // array, $b24.actions.v3.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `pagination` variant when sort matters.
      const response = await $b24.actions.v3.call.make<TaskListResult>({
        method: 'tasks.task.list',
        params: {
          select: [
            'id',
            'title',
            'responsibleId',
            'deadline',
            'status',
          ],
          filter: [
            ['id', '>', 1000],
          ],
          order: {
            id: 'ASC',
          },
          pagination: {
            page: 1,
            limit: 20,
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
        console.info('Tasks:', result.items)
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
      async function listTasks() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'tasks.task.list',
            params: {
              select: [
                'id',
                'title',
                'responsibleId',
                'deadline',
                'status',
              ],
              filter: [
                ['id', '>', 1000],
              ],
              order: {
                id: 'ASC',
              },
              pagination: {
                page: 1,
                limit: 20,
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
          console.info('Tasks:', result.items)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listTasks)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.list',
                [
                    'select' => [
                        'id',
                        'title',
                        'responsibleId',
                        'deadline',
                        'status',
                    ],
                    'filter' => [
                        ['id', '>', 1000],
                    ],
                    'order' => [
                        'id' => 'ASC',
                    ],
                    'pagination' => [
                        'page' => 1,
                        'limit' => 20,
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
        echo 'Error fetching tasks: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'tasks.task.list',
        {
            select: [
                'id',
                'title',
                'responsibleId',
                'deadline',
                'status'
            ],
            filter: [
                ['id', '>', 1000]
            ],
            order: {
                id: 'ASC'
            },
            pagination: {
                page: 1,
                limit: 20,
                offset: 0
            }
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
        'tasks.task.list',
        [
            'select' => [
                'id',
                'title',
                'responsibleId',
                'deadline',
                'status'
            ],
            'filter' => [
                ['id', '>', 1000]
            ],
            'order' => [
                'id' => 'ASC'
            ],
            'pagination' => [
                'page' => 1,
                'limit' => 20,
                'offset' => 0
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "items": [
            {
                "id": 3835,
                "title": "Collect feedback",
                "responsibleId": 93,
                "deadline": "2026-12-25T23:00:00+03:00",
                "status": "pending"
            },
            {
                "id": 3836,
                "title": "Prepare the contract",
                "responsibleId": 29,
                "deadline": null,
                "status": "in_progress"
            },
            {
                "id": 3837,
                "title": "Approve the estimate",
                "responsibleId": 171,
                "deadline": "2025-12-27T18:00:00+03:00",
                "status": "completed"
            }
        ]
    },
    "time": {
        "start": 1765445133,
        "finish": 1765445134.139558,
        "duration": 1.1395580768585205,
        "processing": 1,
        "date_start": "2026-06-17T12:25:33+03:00",
        "date_finish": "2026-06-17T12:25:34+03:00",
        "operating_reset_at": 1765445733,
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
|| **items**
[`array`](../../data-types.md) | Array of task objects ||
|| **items[]**
[`object`](../../data-types.md) | Task object. The set of fields depends on the `select` parameter. If `select` is not specified, the method returns only `id`.

See the field descriptions in the [Task object](./fields.md#taskdto) block ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION",
        "message": "Unable to recognize the pagination parameter `{\"limit\":\"abc\"}`"
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check user permissions and scope `tasks` ||
|#

#### Filtering Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `#FIELD#` | In DTO `TaskDto`, the `#FIELD#` field requires the `Filterable` attribute for such a request | Use only the `id` field in the filter ||
|#

#### Sorting Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `order` | Unable to recognize sorting parameter `#ORDER#` | Provide direction `ASC` or `DESC` and a field available for sorting ||
|#

#### Pagination Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `limit`
`offset`
`page` | Unable to recognize pagination parameter `#PAGE#` | Provide numeric values. `limit` must not be equal to `0` ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-task-get.md)
- [{#T}](./tasks-task-update.md)
- [{#T}](./tasks-task-delete.md)
- [{#T}](./tasks-task-field-list.md)
- [{#T}](./fields.md)