# Delete Time Entry task.elapseditem.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method deletes a time entry.

{% note info %}

You can check the permission to delete using the special method [task.elapseditem.isactionallowed](./task-elapsed-item-is-action-allowed.md).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **TASKID*** 
[`integer`](../../data-types.md) | Task identifier.

You can obtain the task identifier when [creating a new task](../tasks-task-add.md) or by using the [get task list](../tasks-task-list.md) method. ||
|| **ITEMID*** 
[`integer`](../../data-types.md) | Time entry identifier.

You can obtain it when [creating a new entry](./task-elapsed-item-add.md) or by using the [get time entry list](./task-elapsed-item-get-list.md) method. ||
|#

{% note warning %}

It is mandatory to follow the specified order of parameters in the request as shown in the table. Otherwise, the request will execute with errors.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID" : 691, "ITEMID": 5}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.elapseditem.delete
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID" : 691, "ITEMID": 5,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.elapseditem.delete
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // The server returns result:null on successful delete
    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DeleteElapsedItemResult = null

    try {
      const response = await $b24.actions.v2.call.make<DeleteElapsedItemResult>({
        method: 'task.elapseditem.delete',
        params: {
          TASKID: 691,
          ITEMID: 5,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Elapsed item deleted successfully, result:', result)
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
      async function deleteElapsedItem() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.elapseditem.delete',
            params: {
              TASKID: 691,
              ITEMID: 5,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Elapsed item deleted successfully, result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteElapsedItem)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.elapseditem.delete',
                [
                    'TASKID' => 691,
                    'ITEMID' => 5,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting elapsed item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.elapseditem.delete',
        {
            "TASKID": 691,
            "ITEMID": 5,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.elapseditem.delete',
        [
            'TASKID' => 691,
            'ITEMID' => 5,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

In case of a successful request, the server will return `result:null`.

```json
{
    "result": null,
    "time": {
        "start": 1712137817.343984,
        "finish": 1712137817.605804,
        "duration": 0.26182007789611816,
        "processing": 0.018325090408325195,
        "date_start": "2024-04-03T12:50:17+02:00",
        "date_finish": "2024-04-03T12:50:17+02:00"
    }
}
```

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "ACTION_NOT_ALLOWED"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** ||
|| `0x000001` | Task not found ||
|| `0x100002` | Access denied ||
|| `0x000004` | Action not allowed ||
|| `0x000040` | Unknown error ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-elapsed-item-add.md)
- [{#T}](./task-elapsed-item-update.md)
- [{#T}](./task-elapsed-item-get.md)
- [{#T}](./task-elapsed-item-get-list.md)
- [{#T}](./task-elapsed-item-is-action-allowed.md)
- [{#T}](./task-elapsed-item-get-manifest.md)