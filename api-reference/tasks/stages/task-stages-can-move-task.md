# Check the Ability to Move Task task.stages.canmovetask

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user for "My Plan" stages
> - any user with access to the group for kanban stages

This method checks whether the current user can move tasks in the specified object.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **entityId*** 
[`integer`](../../data-types.md) | `ID` of the object ||
|| **entityType*** 
[`string`](../../data-types.md) | Type of the object: 
- `U` — user
- `G` — group

In the case of `U` ("My Plan"), the value `true` will only be returned if the `entityId` contains the identifier of the current user ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "entityId": 1,
    "entityType": "U"
    }' \
    https://your-domain.com/rest/_USER_ID_/_CODE_/task.stages.canmovetask
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "entityId": 1,
    "entityType": "U"
    }' \
    https://your-domain.com/rest/task.stages.canmovetask
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
        method: 'task.stages.canmovetask',
        params: {
          entityId: 1,
          entityType: 'U',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Can move task:', result)
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
      async function checkCanMoveTask() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.stages.canmovetask',
            params: {
              entityId: 1,
              entityType: 'U',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Can move task:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', checkCanMoveTask)
    </script>
    ```

- PHP

    ```php
    try {
        $entityId = 1;
        $entityType = 'U';
    
        $response = $b24Service
            ->core
            ->call(
                'task.stages.canmovetask',
                [
                    'entityId' => $entityId,
                    'entityType' => $entityType
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling task.stages.canmovetask: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const entityId = 1;
    const entityType = 'U';
    BX24.callMethod(
        'task.stages.canmovetask',
        {
            entityId: entityId,
            entityType: entityType
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    $entityId = 1;
    $entityType = 'U';

    // executing the request to the REST API
    $result = CRest::call(
        'task.stages.canmovetask',
        [
            'entityId' => $entityId,
            'entityType' => $entityType
        ]
    );

    // Processing the response from Bitrix24
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
[`boolean`](../../data-types.md) | Returns `true` if the current user can move the task.

Otherwise — `false`
|| 
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-stages-add.md)
- [{#T}](./task-stages-update.md)
- [{#T}](./task-stages-get.md)
- [{#T}](./task-stages-move-task.md)
- [{#T}](./task-stages-delete.md)