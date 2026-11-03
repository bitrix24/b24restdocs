# Delete All Checklists of a Task task.checklistitem.deleteall

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit every item of the task checklists who is:
> - a Bitrix24 administrator
> - the task creator or their supervisor
> - the author of the items or their supervisor
> - the assignee or a participant, if their role allows editing checklists
> - a workgroup member with permission to edit the group's tasks

The method `task.checklistitem.deleteall` deletes all checklists of a task together with all their items, item participants, and files.

{% note warning "" %}

If the user lacks permission to edit at least one item, the method returns an error and deletes no items.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TASKID***
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or using the method [getting a list of tasks](../tasks-task-list.md) ||
|#

## Code Examples

{% include [Example Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.checklistitem.deleteall
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.checklistitem.deleteall
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'task.checklistitem.deleteall',
        params: {
          TASKID: 13,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('All checklists deleted:', result)
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
      async function deleteAllChecklists() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.checklistitem.deleteall',
            params: {
              TASKID: 13,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('All checklists deleted:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteAllChecklists)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        # b24pysdk has no ready-made wrapper for task.checklistitem.deleteall, so the method
        # is called directly through bitrix_token.call_method()
        response = bitrix_token.call_method(
            api_method="task.checklistitem.deleteall",
            params={
                "TASKID": 13,
            },
        )
        result = response["result"]
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

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.checklistitem.deleteall',
                [
                    'TASKID' => 13
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting checklists: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.checklistitem.deleteall',
        {
            TASKID: 13
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
        'task.checklistitem.deleteall',
        [
            'TASKID' => 13
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "task.checklistitem.deleteall", b24.Params{
    	"TASKID": 13,
    })
    if err != nil {
    	return fmt.Errorf("task.checklistitem.deleteall: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1793701273,
        "finish": 1793701273.97813,
        "duration": 0.9781301021575928,
        "processing": 0,
        "date_start": "2026-11-03T13:21:13+01:00",
        "date_finish": "2026-11-03T13:21:13+01:00",
        "operating_reset_at": 1793701860,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the task checklists were deleted.

The method also returns `true` if the task has no checklists, for example, on a repeated call. For a Bitrix24 administrator, the method returns `true` even for a nonexistent task. To make sure the task exists, retrieve it using the [tasks.task.get](../tasks-task-get.md) method ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) expected by method ctaskchecklistitem::deleteall(), but not given.; 256/TE/WRONG_ARGUMENTS<br>"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value**  ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) expected by method ctaskchecklistitem::deleteall(), but not given.; 256/TE/WRONG_ARGUMENTS<br> | Required parameter `TASKID` is missing ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskchecklistitem::deleteall() expected to be of type "integer", but given something else.; 256/TE/WRONG_ARGUMENTS<br> | The `TASKID` value is not an integer ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Delete item: action unavailable; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br> | The user lacks permission to edit at least one item of the task checklists. If the task has no checklists, the error occurs when the user lacks permission to edit the task checklists or the task does not exist ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-checklist-item-add.md)
- [{#T}](./task-checklist-item-update.md)
- [{#T}](./task-checklist-item-get.md)
- [{#T}](./task-checklist-item-get-list.md)
- [{#T}](./task-checklist-item-delete.md)
- [{#T}](./task-checklist-item-is-action-allowed.md)
