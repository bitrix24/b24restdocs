# Get Time Entry by ID task.elapseditem.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: a user with read access to the task

The method `task.elapseditem.get` returns a time entry by its ID.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **TASKID*** 
[`integer`](../../data-types.md) | Task ID.

The task ID can be obtained when [creating a new task](../tasks-task-add.md) or by using the [get task list method](../tasks-task-list.md) ||
|| **ITEMID*** 
[`integer`](../../data-types.md) | Time entry ID.

This can be obtained when [creating a new entry](./task-elapsed-item-add.md) or by using the [get time entry list method](./task-elapsed-item-get-list.md) ||
|#

{% note warning "" %}

It is mandatory to follow the specified order of parameters in the request as shown in the table. Otherwise, the request will execute with errors.

{% endnote %}

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":691,"ITEMID":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.elapseditem.get
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID": 691,"ITEMID": 1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.elapseditem.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ElapsedItemResult = {
      ID: string
      TASK_ID: string
      USER_ID: string
      COMMENT_TEXT: string
      SECONDS: string
      MINUTES: string
      SOURCE: string
      CREATED_DATE: ISODate | null
      DATE_START: ISODate | null
      DATE_STOP: ISODate | null
    }

    try {
      const response = await $b24.actions.v2.call.make<ElapsedItemResult>({
        method: 'task.elapseditem.get',
        params: {
          TASKID: 691,
          ITEMID: 1,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.ID, result.SECONDS, result.CREATED_DATE)
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
      async function getElapsedItem() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.elapseditem.get',
            params: {
              TASKID: 691,
              ITEMID: 1,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.ID, result.SECONDS, result.CREATED_DATE)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getElapsedItem)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.task.elapseditem.get(
            task_id=691,
            item_id=1,
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
                'task.elapseditem.get',
                [
                    'TASKID' => 691,
                    'ITEMID' => 1,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting elapsed item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.elapseditem.get',
        {
            "TASKID": 691,
            "ITEMID": 1,
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
        'task.elapseditem.get',
        [
            'TASKID' => 691,
            'ITEMID' => 1,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "task.elapseditem.get", b24.Params{
    	"TASKID": 691,
    	"ITEMID": 1,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("task.elapseditem.get: %w", err)
    }

    var item struct {
    	ID          b24.ID `json:"ID"`
    	TaskID      b24.ID `json:"TASK_ID"`
    	UserID      b24.ID `json:"USER_ID"`
    	CommentText string `json:"COMMENT_TEXT"`
    	Seconds     string `json:"SECONDS"`
    	Minutes     string `json:"MINUTES"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.TaskID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "ID": "1",
        "TASK_ID": "691",
        "USER_ID": "1",
        "COMMENT_TEXT": "1",
        "SECONDS": "3600",
        "MINUTES": "60",
        "SOURCE": "2",
        "CREATED_DATE": "2024-05-16T10:33:00+02:00",
        "DATE_START": "2024-05-16T10:33:15+02:00",
        "DATE_STOP": "2024-05-16T10:33:15+02:00"
    },
    "time":{
        "start":1712137817.343984,
        "finish":1712137817.605804,
        "duration":0.26182007789611816,
        "processing":0.018325090408325195,
        "date_start":"2024-04-03T12:50:17+03:00",
        "date_finish":"2024-04-03T12:50:17+03:00"
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Information about the time entry [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Time entry identifier ||
|| **TASK_ID**
[`string`](../../data-types.md) | Task identifier ||
|| **USER_ID**
[`string`](../../data-types.md) | Entry author identifier ||
|| **COMMENT_TEXT**
[`string`](../../data-types.md) | Comment ||
|| **SECONDS**
[`string`](../../data-types.md) | Time spent in seconds ||
|| **MINUTES**
[`string`](../../data-types.md) | Time spent in minutes ||
|| **SOURCE**
[`string`](../../data-types.md) | Entry source:
- `1` — source is not specified
- `2` — entry was added manually
- `3` — entry was added automatically ||
|| **CREATED_DATE**
[`datetime`](../../data-types.md) | Entry creation date ||
|| **DATE_START**
[`datetime`](../../data-types.md) | Date and time when tracking started ||
|| **DATE_STOP**
[`datetime`](../../data-types.md) | Date and time when tracking ended ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_CORE",
    "error_description":"TASKS_ERROR_EXCEPTION_#512; Check listitem not found or not accessible; 512/TE/ITEM_NOT_FOUND_OR_NOT_ACCESSIBLE"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Internal Code** | **Description** ||
|| `ERROR_CORE` | `0x000100` | A required parameter was not passed or an invalid type was specified ||
|| `ERROR_CORE` | `0x000200` | The task or entry was not found or is unavailable ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-elapsed-item-add.md)
- [{#T}](./task-elapsed-item-update.md)
- [{#T}](./task-elapsed-item-get-list.md)
- [{#T}](./task-elapsed-item-delete.md)
- [{#T}](./task-elapsed-item-is-action-allowed.md)
- [{#T}](./task-elapsed-item-get-manifest.md)
