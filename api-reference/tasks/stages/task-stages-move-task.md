# Move Task from One Stage to Another task.stages.movetask

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user for stages of "My Plan"
> - any user with access to the group for kanban stages

This method moves a task from one stage to another and allows you to change the position of the task within the group's kanban or "My Plan".

The method works as follows:
- If a group stage is provided, the move occurs within the group's kanban.
- If "My Plan" stage is provided, the move occurs within it.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../data-types.md) | Task identifier ||
|| **stageId*** 
[`integer`](../../data-types.md) | `ID` of the stage to which the task should be moved ||
|| **before** 
[`integer`](../../data-types.md) | `ID` of the task before which the task should be placed in the stage ||
|| **after** 
[`integer`](../../data-types.md) | `ID` of the task after which the task should be placed in the stage ||
|#

{% note info %}

The `before` and `after` parameters are mutually exclusive. You must specify either one or the other.

If both parameters are not filled, the task is added to the stage column according to the project or "My Plan" settings.

{% endnote %}

## Code Examples

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 1,
    "stageId": 2,
    "before": 3,
    "after": 4
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/task.stages.movetask
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "id": 1,
    "stageId": 2,
    "before": 3,
    "after": 4
    }' \
    https://your-domain.bitrix24.com/rest/task.stages.movetask
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type MoveTaskResult = boolean

    try {
      const response = await $b24.actions.v2.call.make<MoveTaskResult>({
        method: 'task.stages.movetask',
        params: {
          id: 1,
          stageId: 2,
          before: 3,
          after: 4,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Task moved successfully:', result)
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
      async function moveTask() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.stages.movetask',
            params: {
              id: 1,
              stageId: 2,
              before: 3,
              after: 4,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Task moved successfully:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', moveTask)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.stages.movetask',
                [
                    'id'     => $taskId,
                    'stageId' => $stageId,
                    'before' => 3,
                    'after'  => 4
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error moving task stage: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const taskId = 1;
    const stageId = 2;
    BX24.callMethod(
        'task.stages.movetask',
        {
            id: taskId,
            stageId: stageId,
            before: 3,
            after: 4
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php'); // include CRest PHP SDK

    $taskId = 1;
    $stageId = 2;

    // execute request to REST API
    $result = CRest::call(
        'task.stages.movetask',
        [
            'id' => $taskId,
            'stageId' => $stageId,
            'before' => 3,
            'after' => 4
        ]
    );

    // Process the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result** 
[`boolean`](../../data-types.md) | Returns `true` if the stage move was successful
||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED_MOVE",
    "error_description": "You cannot move this task"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** ||
|| `ACCESS_DENIED_MOVE` | You cannot move this task ||
|| `TASK_NOT_FOUND` | Task not found or access to it is denied ||
|| `NOT_FOUND` | Stage not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-stages-add.md)
- [{#T}](./task-stages-update.md)
- [{#T}](./task-stages-get.md)
- [{#T}](./task-stages-can-move-task.md)
- [{#T}](./task-stages-delete.md)