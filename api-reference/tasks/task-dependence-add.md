# Create a Link Between Tasks task.dependence.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.dependence.add` creates a dependency between two tasks in the Gantt chart: the `taskIdTo` task becomes dependent on the `taskIdFrom` task.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **taskIdFrom*** 
[`integer`](../data-types.md) | Identifier of the task that the other task depends on ||
|| **taskIdTo*** 
[`integer`](../data-types.md) | Identifier of the dependent task ||
|| **linkType*** 
[`integer`](../data-types.md) | Dependency type:
- `0` — start-to-start: the `taskIdTo` task cannot start before the `taskIdFrom` task starts
- `1` — start-to-finish: the `taskIdTo` task cannot finish before the `taskIdFrom` task starts
- `2` — finish-to-start: the `taskIdTo` task cannot start before the `taskIdFrom` task finishes
- `3` — finish-to-finish: the `taskIdTo` task cannot finish before the `taskIdFrom` task finishes

The method does not validate the value: pass only the numbers from this list ||
|#

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by using the [get task list](./tasks-task-list.md) method.

Two tasks can have only one dependency of any type and direction. A dependency of a task on itself or a repeated dependency between the same pair of tasks returns a circular dependency error. To change the dependency type, delete the dependency using the [task.dependence.delete](./task-dependence-delete.md) method and create it again.

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskIdFrom":100,"taskIdTo":101,"linkType":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.dependence.add
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskIdFrom":100,"taskIdTo":101,"linkType":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.dependence.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (empty array on success)
    type DependenceAddResult = unknown[]

    try {
      const response = await $b24.actions.v2.call.make<DependenceAddResult>({
        method: 'task.dependence.add',
        params: {
          taskIdFrom: 100,
          taskIdTo: 101,
          linkType: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Dependence added, result:', result)
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
      async function addTaskDependence() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.dependence.add',
            params: {
              taskIdFrom: 100,
              taskIdTo: 101,
              linkType: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Dependence added, result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addTaskDependence)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.task.dependence.add(
            task_id_from=100,
            task_id_to=101,
            link_type=0,
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
                'task.dependence.add',
                [
                    'taskIdFrom' => 100,
                    'taskIdTo'   => 101,
                    'linkType'   => 0,
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
        echo 'Error adding task dependence: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.dependence.add', {
            "taskIdFrom":100,
            "taskIdTo":101,
            "linkType":0
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

    $result = CRest::call('task.dependence.add', [
        'taskIdFrom' => 100,
        'taskIdTo' => 101,
        'linkType' => 0,
    ]);

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "task.dependence.add", b24.Params{
    	"taskIdFrom": 100,
    	"taskIdTo":   101,
    	"linkType":   0,
    })
    if err != nil {
    	return fmt.Errorf("task.dependence.add: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [],
    "time": {
        "start": 1795106720,
        "finish": 1795106721.263738,
        "duration": 1.2637379169464111,
        "processing": 1,
        "date_start": "2026-11-19T19:45:20+01:00",
        "date_finish": "2026-11-19T19:45:21+01:00",
        "operating_reset_at": 1795107300,
        "operating": 0.2896251678466797
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | An empty array `[]` if the dependency is created ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACTION_FAILED",
    "error_description": "Circular dependencies are not allowed. (4363, 4361)"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ACTION_FAILED` | Circular dependencies are not allowed. (`taskIdTo`, `taskIdFrom`) | The tasks already have a dependency of any type or direction, `taskIdFrom` and `taskIdTo` are the same, or the dependency would close a dependency chain ||
|| `400` | `ACTION_FAILED` | Task not found | The `taskIdTo` task is not found ||
|| `400` | `ACTION_NOT_ALLOWED.RESTRICTED` | This task action is not allowed | One of the tasks is not found or the user does not have permission to read it ||
|| `400` | `DISPATCHER_ERROR` | Argument "`parameter name`" is required, but no value passed for task.dependence.add | The required `taskIdFrom`, `taskIdTo`, or `linkType` parameter is not provided ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-dependence-delete.md)