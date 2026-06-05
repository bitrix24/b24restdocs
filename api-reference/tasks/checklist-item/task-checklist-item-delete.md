# Delete Checklist Item in task.checklistitem.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user with edit access to the task
> - Creator, Participant, and other Participants of the task

The method `task.checklistitem.delete` removes a checklist item from a task.

You can check permissions for deleting an item using the method [task.checklistitem.isactionallowed](./task-checklist-item-is-action-allowed.md).

## Method Parameters

{% include [Required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **TASKID*** 
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or using the method [getting a list of tasks](../tasks-task-list.md) ||
|| **ITEMID*** 
[`integer`](../../data-types.md) | Checklist item identifier.

The item identifier can be obtained when [adding a new item](./task-checklist-item-add.md) or using the method [getting a list of checklist items](./task-checklist-item-get-list.md) ||
|#

## Code Examples

{% include [Required parameters in examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"ITEMID":475}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.checklistitem.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":13,"ITEMID":475,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.checklistitem.delete
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
        method: 'task.checklistitem.delete',
        params: {
          TASKID: 13,
          ITEMID: 475,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Checklist item deleted:', result)
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
      async function deleteChecklistItem() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.checklistitem.delete',
            params: {
              TASKID: 13,
              ITEMID: 475,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Checklist item deleted:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteChecklistItem)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.checklistitem.delete',
                [
                    'TASKID' => 13,
                    'ITEMID' => 475
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting checklist item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.checklistitem.delete',
        {
            TASKID: 13,
            ITEMID: 475
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
        'task.checklistitem.delete',
        [
            'TASKID' => 13,
            'ITEMID' => 475
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
    "result": true,
    "time": {
        "start": 1764594534,
        "finish": 1764594535.201817,
        "duration": 1.2018170356750488,
        "processing": 0,
        "date_start": "2025-12-01T16:08:54+01:00",
        "date_finish": "2025-12-01T16:08:55+01:00",
        "operating_reset_at": 1764595135,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the checklist item was successfully deleted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "TASKS_ERROR_EXCEPTION_#8; Deleting item: action not allowed; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br>"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** | **Value**  ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #1 (itemId) expected by method ctaskchecklistitem::delete(), but not given.; 256/TE/WRONG_ARGUMENTS<br> | Required parameter `TASKID` or `ITEMID` is missing ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskchecklistitem::delete() expected to be of type "integer", but given something else.; 256/TE/WRONG_ARGUMENTS<br> | Incorrect value type for `TASKID` or `ITEMID` ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Deleting item: action not allowed; 8/TE/ACTION_FAILED_TO_BE_PROCESSED<br> | User does not have access rights to the task or lacks permissions to perform the action ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-checklist-item-add.md)
- [{#T}](./task-checklist-item-update.md)
- [{#T}](./task-checklist-item-get.md)
- [{#T}](./task-checklist-item-get-list.md)
- [{#T}](./task-checklist-item-move-after-item.md)
- [{#T}](./task-checklist-item-complete.md)
- [{#T}](./task-checklist-item-renew.md)
- [{#T}](./task-checklist-item-is-action-allowed.md)