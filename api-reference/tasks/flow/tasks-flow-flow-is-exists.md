# Check the Existence of tasks.flow.Flow.isExists

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.flow.Flow.isExists` checks whether a flow with the specified name exists. If an `id` is provided, it checks for flows with the same name, excluding the specified one.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **flowData*** 
[`object`](../../data-types.md) | Object containing data to check for flow existence ||
|| **name*** 
[`string`](../../data-types.md) | The name of the flow to check ||
|| **id** 
[`integer`](../../data-types.md) | Identifier of the flow to exclude from the check (optional). 

You can obtain the identifier using the method for creating a new flow [tasks.flow.Flow.create](./tasks-flow-flow-create.md) or by retrieving a task [tasks.task.get](../tasks-task-get.md) for a task from the flow ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
        "flowData": {
            "name": "Flow Name"
        }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.flow.Flow.isExists
    ```

- cURL (oAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
    -d '{
        "flowData": {
            "name": "Flow Name"
        }
    }' \
    https://your-domain.bitrix24.com/rest/tasks.flow.Flow.isExists
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FlowIsExistsResult = {
      exists: boolean
    }

    try {
      const response = await $b24.actions.v2.call.make<FlowIsExistsResult>({
        method: 'tasks.flow.Flow.isExists',
        params: {
          flowData: {
            name: 'Flow Name',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Flow exists:', result.exists)
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
      async function checkFlowIsExists() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.flow.Flow.isExists',
            params: {
              flowData: {
                name: 'Flow Name',
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
          console.info('Flow exists:', result.exists)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', checkFlowIsExists)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.flow.Flow.isExists',
                [
                    'flowData' => [
                        'name' => 'Flow Name'
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Info: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error checking flow existence: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.flow.Flow.isExists',
        {
            flowData: {
                name: 'Flow Name'
            }
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
    require_once('crest.php'); // connecting CRest PHP SDK

    $flowData = [
        "name" => "Flow Name"
    ];

    // executing a request to the REST API
    $result = CRest::call(
        'tasks.flow.Flow.isExists',
        [
            'flowData' => $flowData
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
    "result": {
        "exists": true
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result** 
[`object`](../../data-types.md) | Object containing the result of the operation ||
|| **exists** 
[`boolean`](../../data-types.md) | Indicates whether a flow with the specified name exists ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Flow not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description** | **Additional Information** ||
|| `0` | Access denied or flow not found | The account plan does not allow working with flows or the user does not have permission to perform the check ||
|| `0` | `Unknown error` | An unknown error occurred ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-flow-flow-create.md)
- [{#T}](./tasks-flow-flow-update.md)
- [{#T}](./tasks-flow-flow-get.md)
- [{#T}](./tasks-flow-flow-delete.md)
- [{#T}](./tasks-flow-flow-activate.md)
- [{#T}](./tasks-flow-flow-pin.md)