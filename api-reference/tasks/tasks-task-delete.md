# Delete Task tasks.task.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user with permission to delete a task

The method `tasks.task.delete` deletes a task.

You can check the permission to delete a task using the [check access to task](./tasks-task-get-access.md) method.

## Method Parameters

{% include [Note on parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **taskId***
[`integer`](../data-types.md) | The identifier of the task.

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or using the [get task list](./tasks-task-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8131}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8131,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.delete
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // The API wraps the deletion flag: result.task === true on success
    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TaskDeleteResult = {
      task: boolean
    }

    try {
      const response = await $b24.actions.v2.call.make<TaskDeleteResult>({
        method: 'tasks.task.delete',
        params: {
          taskId: 8131 // ID of the task to delete
        },
        requestId: Text.getUuidRfc4122() // optional unique tracking id for this request
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else if (response.getData()!.result.task) {
        console.info('Task deleted successfully')
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
      async function deleteTask() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.task.delete',
            params: {
              taskId: 8131 // ID of the task to delete
            },
            requestId: B24Js.Text.getUuidRfc4122() // optional unique tracking id for this request
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          if (response.getData().result.task) {
            console.info('Task deleted successfully')
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteTask)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.delete',
                [
                    'taskId' => 8131
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting task: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.task.delete',
        {
            'taskId': 8131
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.delete',
        [
            'taskId' => 8131
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
        "task": true
    },
    "time": {
        "start": 1758184832.67041,
        "finish": 1758184832.961555,
        "duration": 0.29114508628845215,
        "processing": 0.2604410648345947,
        "date_start": "2025-09-18T11:40:32+02:00",
        "date_finish": "2025-09-18T11:40:32+02:00",
        "operating_reset_at": 1758185432,
        "operating": 0.2604219913482666
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Root element of the response ||
|| **task**
[`boolean`](../data-types.md) | Returns `true` if the task was successfully deleted ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"1048582",
    "error_description":"No access to delete the task"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | wrong task id | The `taskId` parameter has an invalid type ||
|| `1048582` | No access to delete the task | The user does not have access to the task or lacks permission to delete the task ||
|| `100` | CTaskItem All parameters in the constructor must have real class type | The required parameter `taskId` is missing ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-task-add.md)
- [{#T}](./tasks-task-update.md)
- [{#T}](./tasks-task-get.md)
- [{#T}](./tasks-task-list.md)
- [{#T}](./tasks-task-get-fields.md)