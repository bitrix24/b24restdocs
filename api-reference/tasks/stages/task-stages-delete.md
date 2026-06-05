# Delete a Kanban or "My Plan" Stage task.stages.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user for "My Plan" stages
> - any user with group access for Kanban stages

This method deletes a Kanban or "My Plan" stage.

It accepts the `id` of the stage as input. The stage is checked for sufficient access permission levels and whether it contains any tasks.

## Method Parameters

{% include [Required parameters note](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the stage to be deleted ||
|| **isAdmin**
[`boolean`](../../data-types.md) | If set to `true`, permission checks will be skipped, provided the requester is an administrator of the account ||
|#

## Code Examples

{% include [Examples note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 5
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/task.stages.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "id": 5
    }' \
    https://your-domain.bitrix24.com/rest/task.stages.delete
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
        method: 'task.stages.delete',
        params: {
          id: 5,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Stage deleted:', result)
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
      async function deleteStage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.stages.delete',
            params: {
              id: 5,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Stage deleted:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteStage)
    </script>
    ```

- PHP

    ```php
    try {
        $stageId = 5;
        $response = $b24Service
            ->core
            ->call(
                'task.stages.delete',
                [
                    'id' => $stageId,
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
        echo 'Error deleting task stage: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const stageId = 5;
    BX24.callMethod(
        'task.stages.delete',
        {
            id: stageId,
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

    $stageId = 5;

    // execute request to REST API
    $result = CRest::call(
        'task.stages.delete',
        [
            'id' => $stageId
        ]
    );

    // Process response from Bitrix24
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
[`boolean`](../../data-types.md) | Returns `true` if the stage was successfully deleted
||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "CANT_DELETE_FIRST",
    "error_description": "Cannot delete the first stage. Move the stage to delete it."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | You cannot manage stages ||
|| `CANT_DELETE_FIRST` | Cannot delete the first stage. Move the stage to delete it ||
|| `IS_SYSTEM` | The default stage cannot be deleted ||
|| `NOT_FOUND` | Stage not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-stages-add.md)
- [{#T}](./task-stages-update.md)
- [{#T}](./task-stages-get.md)
- [{#T}](./task-stages-can-move-task.md)
- [{#T}](./task-stages-move-task.md)