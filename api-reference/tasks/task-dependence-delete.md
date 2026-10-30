# Remove Dependency Between Tasks task.dependence.delete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.dependence.delete` deletes a dependency between two tasks in the Gantt chart. The tasks themselves are not changed.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **taskIdFrom***
[`integer`](../data-types.md) | Identifier of the task that the second task depends on. The same value that was passed in `taskIdFrom` when creating the dependency with the [task.dependence.add](./task-dependence-add.md) method ||
|| **taskIdTo***
[`integer`](../data-types.md) | Identifier of the dependent task. The same value that was passed in `taskIdTo` when creating the dependency.

The order matters: if you swap `taskIdFrom` and `taskIdTo`, the method does not find the dependency and returns an error ||
|#

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by using the [method for retrieving the list of tasks](./tasks-task-list.md).

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskIdFrom":100,"taskIdTo":101}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.dependence.delete
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskIdFrom":100,"taskIdTo":101,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.dependence.delete
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (empty array on success)
    type DeleteDependenceResult = never[]

    try {
      const response = await $b24.actions.v2.call.make<DeleteDependenceResult>({
        method: 'task.dependence.delete',
        params: {
          taskIdFrom: 100,
          taskIdTo: 101,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Dependency deleted successfully, result:', result)
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
      async function deleteDependence() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.dependence.delete',
            params: {
              taskIdFrom: 100,
              taskIdTo: 101,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Dependency deleted successfully, result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteDependence)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.task.dependence.delete(
            task_id_from=100,
            task_id_to=101,
        ).response
        result = bitrix_response.result
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
                'task.dependence.delete',
                [
                    'taskIdFrom' => 100,
                    'taskIdTo'   => 101,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.dependence.delete', {
            "taskIdFrom":100,
            "taskIdTo":101,
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
        'task.dependence.delete',
        [
            'taskIdFrom' => 100,
            'taskIdTo' => 101,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "task.dependence.delete", b24.Params{
    	"taskIdFrom": 100,
    	"taskIdTo":   101,
    })
    if err != nil {
    	return fmt.Errorf("task.dependence.delete: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [],
    "time": {
        "start": 1793277236,
        "finish": 1793277237.005083,
        "duration": 1.0050830841064453,
        "processing": 1,
        "date_start": "2026-10-29T15:33:56+01:00",
        "date_finish": "2026-10-29T15:33:57+01:00",
        "operating_reset_at": 1793277780,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | An empty array `[]` if the dependency is deleted ||
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ACTION_FAILED",
    "error_description": "Link does not exist (nodes: 4265, 4263)"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ACTION_FAILED` | Link does not exist (nodes: `taskIdTo`, `taskIdFrom`) | There is no dependency between the tasks: it has already been deleted, was never created, or `taskIdFrom` and `taskIdTo` are swapped ||
|| `400` | `ACTION_NOT_ALLOWED.RESTRICTED` | This task action is not allowed | The user does not have permission to read one of the tasks ||
|| `400` | `DISPATCHER_ERROR` | Argument "`parameter name`" is required, but no value passed for task.dependence.delete | The required `taskIdFrom` or `taskIdTo` parameter is not provided ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-dependence-add.md)