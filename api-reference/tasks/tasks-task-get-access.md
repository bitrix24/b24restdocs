# Check Access to Task tasks.task.getaccess

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.getaccess` returns the task actions available to each of the specified users.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **taskId*** 
[`integer`](../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by using the [get task list](./tasks-task-list.md) method. ||
|| **users** 
[`array`](../data-types.md) | An array of user identifiers for whom access needs to be checked.

By default, and when an empty array is passed, the current user is checked.

The user identifier can be obtained using the [get user list](../user/user-get.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8017,"users":[503,547]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.getaccess
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8017,"users":[503,547],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.getaccess
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TaskAccessResult = {
      allowedActions: Record<string, Record<string, boolean>>
    }

    try {
      const response = await $b24.actions.v2.call.make<TaskAccessResult>({
        method: 'tasks.task.getaccess',
        params: {
          taskId: 8017,
          users: [503, 547],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Allowed actions by user:', result.allowedActions)
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
      async function getTaskAccess() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.task.getaccess',
            params: {
              taskId: 8017,
              users: [503, 547],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Allowed actions by user:', result.allowedActions)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTaskAccess)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.tasks.task.getaccess(
            task_id=8017,
            users=[
                503,
                547,
            ],
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
                'tasks.task.getaccess',
                [
                    'taskId' => 8017,
                    'users' => [503, 547]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.task.getaccess',
        {
            'taskId': 8017,
            'users': [503, 547]
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
        'tasks.task.getaccess',
        [
            'taskId' => 8017,
            'users' => [503, 547]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "tasks.task.getaccess", b24.Params{
    	"taskId": 8017,
    	"users":  []int{503, 547},
    })
    if err != nil {
    	return fmt.Errorf("tasks.task.getaccess: %w", err)
    }

    // The method wraps the response in an object with the "allowedActions" key.
    raw, ok := b24.Unwrap(res.Result, "allowedActions")
    if !ok {
    	return fmt.Errorf("no allowedActions key in the response")
    }

    fmt.Printf("%s\n", raw)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "allowedActions": {
            "1269": {
                "ACCEPT": false,
                "DECLINE": false,
                "COMPLETE": true,
                "APPROVE": false,
                "DISAPPROVE": false,
                "START": true,
                "PAUSE": false,
                "DELEGATE": true,
                "REMOVE": true,
                "EDIT": true,
                "DEFER": true,
                "RENEW": false,
                "CREATE": true,
                "CHANGE_DEADLINE": true,
                "CHECKLIST_ADD_ITEMS": true,
                "ADD_FAVORITE": true,
                "DELETE_FAVORITE": false,
                "RATE": true,
                "TAKE": false,
                "EDIT.ORIGINATOR": false,
                "CHECKLIST.REORDER": true,
                "ELAPSEDTIME.ADD": true,
                "DAYPLAN.TIMER.TOGGLE": false,
                "EDIT.PLAN": true,
                "CHECKLIST.ADD": true,
                "FAVORITE.ADD": true,
                "FAVORITE.DELETE": false
            }
        }
    },
    "time": {
        "start": 1795106839,
        "finish": 1795106839.631999,
        "duration": 0.6319990158081055,
        "processing": 0,
        "date_start": "2026-11-19T19:47:19+01:00",
        "date_finish": "2026-11-19T19:47:19+01:00",
        "operating_reset_at": 1795107420,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | The root element of the response ||
|| **allowedActions**
[`object`](../data-types.md) \| [`array`](../data-types.md) | An object where the key is the user ID and the value is an object with the [available actions](./fields.md#action) on the task. In the response, action keys are in uppercase: for example, `CHANGE_DEADLINE` corresponds to the `changeDeadline` field in the description.

If the task is not found or the user calling the method has no access to it, the method returns an empty array `"allowedActions": []` ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time. ||
|#

The `ACCEPT` and `DECLINE` actions are deprecated and are always `false`.

For a non-existent user in `users`, the method does not return an error: all actions except `CREATE` are `false`.

## Error Handling

HTTP Status: **400**

```json
{
    "error": "100",
    "error_description": "Invalid value {1269} to match with parameter {users}. Should be value of type array."
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for {taskId} | The required `taskId` parameter is not provided ||
|| `400` | `0` | wrong task id | `taskId` is not a number, for example, `abc` ||
|| `400` | `100` | Invalid value {`value`} to match with parameter {users}. Should be value of type array. | `users` is not an array, for example, a string ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-task-get.md)
- [{#T}](./tasks-task-list.md)
- [{#T}](./fields.md)