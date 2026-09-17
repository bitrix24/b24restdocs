# How to Execute Batch Requests in REST 3.0

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`basic`](../../api-reference/scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../api-reference/rest-v3.md).

{% endnote %}

The REST 3.0 `batch` method executes multiple requests in a single API call. You can pass the result of a previous subrequest to the parameters of the next one.

Method endpoint:

```http
POST https://{installation_address}/rest/api/{user_id}/{webhook_token}/batch
```

Pass the request body in JSON format as an array of subrequest objects.

## When to Use batch

The method supports two scenarios:

- execute multiple independent methods in a single server call
- pass the result of one subrequest to the parameters of the next one

## Method Parameters

The method accepts a JSON array of subrequests in the request body. Each array element is an object that describes one call.

### Batch Array Element

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **method***
[`string`](../../api-reference/data-types.md) | Name of the method to call ||
|| **query***
[`object`](../../api-reference/data-types.md) | Parameters of the method to call. If the method has no parameters, pass an empty object `{}` ||
|| **as**
[`string`](../../api-reference/data-types.md) | Unique name of the subrequest result. Use this name to reference the result in subsequent subrequests.

If `as` is not specified, reference the result by the subrequest index. Indexing starts at zero.

If two subrequests have the same name, batch returns the `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION` error ||
|#

### Passing a Single Value

To pass a value from a previous subrequest result, use an object with the `$ref` key:

```json
{"$ref": "first_task.id"}
```

The path consists of the subrequest identifier and the dot-separated path to the field. The identifier can be an `as` value or a subrequest index, such as `1.id`.

### Passing an Array of Values

To collect the values of one field from all result items, use `$refArray`:

```json
{"$refArray": "tasks_list.id"}
```

In this example, the API takes the `tasks_list` subrequest result, extracts the `id` field from each item, and passes the resulting array to the next request.

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/batch`

{% endnote %}

You can call batch 3.0 using a direct HTTP request or B24JsSDK.

The `JS (TS)` and `JS (UMD)` examples require B24JsSDK version 2.0 or later.

### Independent Calls

If the subrequest results are not related, pass them in a single array without `$ref` and `$refArray` references:

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '[
        {"method":"tasks.task.get","query":{"id":101,"select":["id","title"]}},
        {"method":"tasks.task.get","query":{"id":102,"select":["id","title"]}}
    ]' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/batch
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type TaskGetResult = {
      item: {
        id: number
        title: string
      }
    }

    try {
      const response = await $b24.actions.v3.batch.make<TaskGetResult>({
        calls: [
          {
            method: 'tasks.task.get',
            params: {
              id: 101,
              select: ['id', 'title'],
            },
          },
          {
            method: 'tasks.task.get',
            params: {
              id: 102,
              select: ['id', 'title'],
            },
          },
        ],
        options: {
          isHaltOnError: true,
          returnAjaxResult: true,
          requestId: Text.getUuidRfc4122(),
        },
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const results = response.getData()!
        console.info(results.map((item) => item.getData()!.result.item))
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@2/dist/umd/index.min.js"></script>
    <script>
      async function getTasks() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.batch.make({
            calls: [
              {
                method: 'tasks.task.get',
                params: {
                  id: 101,
                  select: ['id', 'title'],
                },
              },
              {
                method: 'tasks.task.get',
                params: {
                  id: 102,
                  select: ['id', 'title'],
                },
              },
            ],
            options: {
              isHaltOnError: true,
              returnAjaxResult: true,
              requestId: B24Js.Text.getUuidRfc4122(),
            },
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const results = response.getData()
          console.info(results.map((item) => item.getData().result.item))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTasks)
    </script>
    ```

{% endlist %}

Each result in the response corresponds to the subrequest with the same index.

### Sequential Execution

The subrequests are executed in the following order:

1. [tasks.task.get](../../api-reference/tasks/tasks-task-get-rest-v3.md) retrieves a task and stores the result under the name `first_task`
2. The second `tasks.task.get` call retrieves another task. The subrequest has no name, so its result is referenced by the index `1`
3. [tasks.task.update](../../api-reference/tasks/tasks-task-update-rest-v3.md) retrieves the task ID from `first_task` through `$ref` and updates its title
4. [tasks.task.list](../../api-reference/tasks/tasks-task-list-rest-v3.md) retrieves tasks by their IDs and stores the list under the name `tasks_list`
5. The second `tasks.task.list` call retrieves all IDs from `tasks_list` through `$refArray`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '[
        {"method":"tasks.task.get","query":{"id":101,"select":["id","title"]},"as":"first_task"},
        {"method":"tasks.task.get","query":{"id":102,"select":["id","title"]}},
        {"method":"tasks.task.update","query":{"id":{"$ref":"first_task.id"},"fields":{"title":"Updated task"}}},
        {"method":"tasks.task.list","query":{"select":["id","title"],"filter":[["id","in",[101,{"$ref":"first_task.id"},{"$ref":"1.id"}]]]},"as":"tasks_list"},
        {"method":"tasks.task.list","query":{"select":["id","title"],"filter":[["id","in",{"$refArray":"tasks_list.id"}]]}}
    ]' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/batch
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { BatchRefV3, Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type TaskGetResult = {
      item: {
        id: number
        title: string
      }
    }

    type TaskUpdateResult = {
      result: boolean
    }

    type TaskListResult = {
      items: Array<{
        id: number
        title: string
      }>
    }

    try {
      const response = await $b24.actions.v3.batch.make<
        TaskGetResult | TaskUpdateResult | TaskListResult
      >({
        calls: [
          {
            method: 'tasks.task.get',
            params: {
              id: 101,
              select: ['id', 'title'],
            },
            as: 'first_task',
          },
          {
            method: 'tasks.task.get',
            params: {
              id: 102,
              select: ['id', 'title'],
            },
          },
          {
            method: 'tasks.task.update',
            params: {
              id: BatchRefV3.ref('first_task.id'),
              fields: {
                title: 'Updated task',
              },
            },
          },
          {
            method: 'tasks.task.list',
            params: {
              select: ['id', 'title'],
              filter: [
                ['id', 'in', [
                  101,
                  BatchRefV3.ref('first_task.id'),
                  BatchRefV3.ref('1.id'),
                ]],
              ],
            },
            as: 'tasks_list',
          },
          {
            method: 'tasks.task.list',
            params: {
              select: ['id', 'title'],
              filter: [
                ['id', 'in', BatchRefV3.refArray('tasks_list.id')],
              ],
            },
          },
        ],
        options: {
          isHaltOnError: true,
          returnAjaxResult: true,
          requestId: Text.getUuidRfc4122(),
        },
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const results = response.getData()!
        console.info(results.map((item) => item.getData()!.result))
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@2/dist/umd/index.min.js"></script>
    <script>
      async function runTaskBatch() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.batch.make({
            calls: [
              {
                method: 'tasks.task.get',
                params: {
                  id: 101,
                  select: ['id', 'title'],
                },
                as: 'first_task',
              },
              {
                method: 'tasks.task.get',
                params: {
                  id: 102,
                  select: ['id', 'title'],
                },
              },
              {
                method: 'tasks.task.update',
                params: {
                  id: B24Js.BatchRefV3.ref('first_task.id'),
                  fields: {
                    title: 'Updated task',
                  },
                },
              },
              {
                method: 'tasks.task.list',
                params: {
                  select: ['id', 'title'],
                  filter: [
                    ['id', 'in', [
                      101,
                      B24Js.BatchRefV3.ref('first_task.id'),
                      B24Js.BatchRefV3.ref('1.id'),
                    ]],
                  ],
                },
                as: 'tasks_list',
              },
              {
                method: 'tasks.task.list',
                params: {
                  select: ['id', 'title'],
                  filter: [
                    ['id', 'in', B24Js.BatchRefV3.refArray('tasks_list.id')],
                  ],
                },
              },
            ],
            options: {
              isHaltOnError: true,
              returnAjaxResult: true,
              requestId: B24Js.Text.getUuidRfc4122(),
            },
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const results = response.getData()
          console.info(results.map((item) => item.getData().result))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', runTaskBatch)
    </script>
    ```

{% endlist %}

{% note warning "" %}

REST 3.0 does not support the format with a `cmd` object and strings such as `method?param=value`. Pass subrequests as a JSON array of objects.

{% endnote %}

## Response Handling

HTTP status of a successful response: **200**.

```json
{
    "result": [
        {
            "item": {
                "id": 101,
                "title": "First task"
            }
        },
        {
            "item": {
                "id": 102,
                "title": "Second task"
            }
        },
        {
            "result": true
        },
        {
            "items": [
                {
                    "id": 101,
                    "title": "Updated task"
                },
                {
                    "id": 102,
                    "title": "Second task"
                }
            ]
        },
        {
            "items": [
                {
                    "id": 101,
                    "title": "Updated task"
                },
                {
                    "id": 102,
                    "title": "Second task"
                }
            ]
        }
    ],
    "time": {
        "start": 1750096028,
        "finish": 1750096028.292702,
        "duration": 0.29270195960998535,
        "processing": 0,
        "date_start": "2025-06-16T17:47:08+00:00",
        "date_finish": "2025-06-16T17:47:08+00:00"
    }
}
```

## Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../api-reference/data-types.md) | Array of subrequest results in execution order ||
|| **result[n]**
[`object`](../../api-reference/data-types.md) | Result of the subrequest with the index `n`. Indexing starts at zero.

Method data is located in `item`, `items`, or `result`, depending on the called method ||
|| **result[].item**
[`object`](../../api-reference/data-types.md) | Result of a method that returns one object ||
|| **result[].items**
[`array`](../../api-reference/data-types.md) | Result of a method that returns a list of objects ||
|| **result[].result**
[`boolean`](../../api-reference/data-types.md) | Operation result when the method returns a success indicator ||
|| **time**
[`time`](../../api-reference/data-types.md#time) | Information about the batch execution time ||
|#

## Error Handling

If an error occurs in the batch request itself or in a nested call, batch returns an `error` object at the top level and does not return an array of successful results. Check the HTTP status and error code. The general error format is described in the [REST 3.0 overview](../../api-reference/rest-v3.md#response).

Example of an error when referencing a nonexistent path in `$ref`:

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION",
        "message": "Unable to parse select expression `Path 'first_task.item.id' not found in context`"
    }
}
```

{% include notitle [error handling](../../_includes/error-info-v3.md) %}

### Possible Error Codes

#|
|| **Code** | **HTTP Status** | **Cause** | **What to Check** ||
|| `BITRIX_REST_V3_EXCEPTION_INVALIDJSONEXCEPTION` | 400 | The request body is not valid JSON | Check brackets, quotation marks, and Content-Type `application/json` ||
|| `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION` | 400 | The nested method parameters failed validation | Check `query` and the requirements on the nested method page ||
|| `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION` | 400 | The `$ref` reference or `select` expression could not be parsed, or an `as` name is duplicated | Check the subrequest name, field path, and uniqueness of `as` ||
|| `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION` | 400 | The nested method did not find an object with the specified ID | Check the object ID ||
|| `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION` | 403 | The user does not have access to the object | Check the user permissions ||
|| `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION` | 403 | The webhook or application does not have the required scope | Add the scope required by the nested method ||
|| `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION` | 400 | The batch specifies a method that does not exist in the API | Check the nested method name and version ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Limitations

- Subrequests are executed sequentially only
- A nested `batch` call is not supported

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./batch.md)
- [{#T}](../../api-reference/rest-v3.md)
- [{#T}](../../api-reference/tasks/tasks-task-get-rest-v3.md)
- [{#T}](../../api-reference/tasks/tasks-task-update-rest-v3.md)
- [{#T}](../../api-reference/tasks/tasks-task-list-rest-v3.md)
