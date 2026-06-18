# Add Result to Task tasks.task.result.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../../ai-tools/mcp.md) so the assistant can use the official REST documentation.

{% endnote %}

> Scope: [`tasks`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to the task

The method `tasks.task.result.add` adds a result to a task.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with [result fields](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **taskId***
[`integer`](../../../data-types.md) | Task identifier.

The identifier can be obtained when [creating a task](../tasks-task-add.md), [getting a task](../tasks-task-get.md), or using the old method of [getting a list of tasks](../../../tasks/tasks-task-list.md) ||
|| **text***
[`string`](../../../data-types.md) | Result text ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.task.result.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"taskId":51,"text":"Task completed"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.result.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"taskId":51,"text":"Task completed"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.task.result.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TaskResultAddResult = {
      item: {
        id: number,
        taskId: number,
        text: string,
        authorId: number,
        createdAt: ISODate,
        updatedAt: ISODate | null,
        status: string,
        fileIds: number[],
        rights: {
          edit: boolean,
          remove: boolean,
        },
        messageId: number | null,
      },
    }

    try {
      const response = await $b24.actions.v3.call.make<TaskResultAddResult>({
        method: 'tasks.task.result.add',
        params: {
          fields: {
            taskId: 51,
            text: 'Task work has been completed',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item.id, result.item.taskId, result.item.status)
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
      async function addTaskResult() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'tasks.task.result.add',
            params: {
              fields: {
                taskId: 51,
                text: 'Task work has been completed',
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
          console.info(result.item.id, result.item.taskId, result.item.status)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addTaskResult)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.result.add',
                [
                    'fields' => [
                        'taskId' => 51,
                        'text' => 'Task completed',
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'tasks.task.result.add',
        {
            fields: {
                taskId: 51,
                text: 'Task completed'
            }
        },
        function(result){
            console.info(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.result.add',
        [
            'fields' => [
                'taskId' => 51,
                'text' => 'Task completed',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "item": {
            "id": 17,
            "taskId": 51,
            "text": "Task completed",
            "authorId": 1,
            "createdAt": "2026-04-30T10:15:00+02:00",
            "updatedAt": null,
            "status": "open",
            "fileIds": [],
            "rights": {
                "edit": true,
                "remove": true
            },
            "messageId": null
        }
    },
    "time": {
        "start": 1777529700,
        "finish": 1777529700.123456,
        "duration": 0.123456,
        "processing": 0.1,
        "date_start": "2026-04-30T10:15:00+02:00",
        "date_finish": "2026-04-30T10:15:00+02:00",
        "operating_reset_at": 1777530300,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **item**
[`object`](../../../data-types.md) | Task result object [(detailed description)](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Object item {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Result identifier ||
|| **taskId**
[`integer`](../../../data-types.md) | Task identifier ||
|| **text**
[`string`](../../../data-types.md) | Result text ||
|| **authorId**
[`integer`](../../../data-types.md) | Result author identifier ||
|| **createdAt**
[`datetime`](../../../data-types.md) | Result creation date ||
|| **updatedAt**
[`datetime`](../../../data-types.md) | Result update date ||
|| **status**
[`string`](../../../data-types.md) | Result status. Possible values: `open`, `closed` ||
|| **fileIds**
[`array`](../../../data-types.md) | Result file identifiers ||
|| **rights**
[`object`](../../../data-types.md) | Current user's rights on the result [(detailed description)](#rights) ||
|| **messageId**
[`integer`](../../../data-types.md) | Chat message identifier if the result was created from a message ||
|#

#### Object rights {#rights}

#|
|| **Name**
`type` | **Description** ||
|| **edit**
[`boolean`](../../../data-types.md) | Returns `true` if the current user can edit the result ||
|| **remove**
[`boolean`](../../../data-types.md) | Returns `true` if the current user can delete the result ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION",
        "message": "Validation error",
        "validation": [
            {
                "field": "text",
                "message": "The required field \"text\" is not filled"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `taskId` | Required field `taskId` is not specified | Provide the task identifier in `fields.taskId` ||
|| `text` | Required field `text` is not specified | Provide the result text in `fields.text` ||
|| `fileIds` | Field `fileIds` is not available for filling | Do not provide `fileIds` in the `fields` parameter ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `fields` | Required field `fields` is not specified | Provide the `fields` object with `taskId` and `text` fields ||
|| `taskId` | Field `taskId` requires `int` data type for this request | Provide `taskId` as a number ||
|| `taskId` | `Task id must be positive` | Provide a positive task identifier ||
|| `taskId` | `Task id must be set` | Provide the task identifier in `fields.taskId` ||
|| `taskId` | `Task not found` | Specify the identifier of an existing task ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP Status: **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `taskId` | Access denied | Check the user's rights on the task ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-task-result-addfromchatmessage.md)
- [{#T}](./tasks-task-result-update.md)
- [{#T}](./tasks-task-result-list.md)
- [{#T}](./tasks-task-result-delete.md)
