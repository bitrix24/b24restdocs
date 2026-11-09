# Update Kanban Stage or "My Planner" task.stages.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user for stages of their own "My Planner"
> - the group owner or moderator for the group's Kanban stages

The method `task.stages.update` changes the name, color, or position of a Kanban or "My Planner" stage.

To move a stage, pass in `AFTER_ID` the ID of the stage after which it should be placed.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../data-types.md) | Identifier of the stage ||
|| **fields*** 
[`object`](../../data-types.md) | Field values for updating the Kanban or "My Planner" stage [(detailed description)](#fields) ||
|| **isAdmin** 
[`boolean`](../../data-types.md) | If `true` is passed, the method does not check permissions in the group. The parameter applies only to a Bitrix24 administrator; for other users, permissions are checked as usual. Default is `false` ||
|#

### Parameter fields {#fields}

#| 
|| **Name**
`type` | **Description** ||
|| **TITLE**
[`string`](../../data-types.md) | Stage name. If not passed, the name stays the same ||
|| **COLOR**
[`string`](../../data-types.md) | Stage color — a 6-character hexadecimal RGB code without `#`, for example `FF5733` ||
|| **AFTER_ID**
[`integer`](../../data-types.md) | Identifier of the stage after which this stage should be placed.

If `0` is passed, the stage becomes the first one. If not passed, the stage stays in place ||
|#

The method silently skips other fields. For example, `SORT` does not change the order of stages: the stage position is set only by `AFTER_ID`.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
    "id": 5,
    "fields": {
        "TITLE": "New Stage",
        "COLOR": "FF5733"
    }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.stages.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
    "id": 5,
    "fields": {
        "TITLE": "New Stage",
        "COLOR": "FF5733"
    },
    "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/task.stages.update
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
        method: 'task.stages.update',
        params: {
          id: 5,
          fields: {
            TITLE: 'New Stage',
            COLOR: 'FF5733',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Stage updated:', result)
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
      async function updateStage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.stages.update',
            params: {
              id: 5,
              fields: {
                TITLE: 'New Stage',
                COLOR: 'FF5733',
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
          console.info('Stage updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateStage)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "TITLE": "New stage",
        "COLOR": "FF5733",
    }

    try:
        bitrix_response = client.task.stages.update(
            bitrix_id=5,
            fields=fields,
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
        $stageId = 5;
        $fields = [
            'TITLE' => "New Stage",
            'COLOR' => "FF5733"
        ];

        $response = $b24Service
            ->core
            ->call(
                'task.stages.update',
                [
                    'id' => $stageId,
                    'fields' => $fields
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating task stage: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const stageId = 5;
    const fields = {
        TITLE: "New Stage",
        COLOR: "FF5733"
    };
    BX24.callMethod(
        'task.stages.update',
        {
            id: stageId,
            fields: fields
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
    $fields = [
        "TITLE" => "New Stage",
        "COLOR" => "FF5733"
    ];

    // execute request to REST API
    $result = CRest::call(
        'task.stages.update',
        [
            'id' => $stageId,
            'fields' => $fields
        ]
    );

    // Handle response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "task.stages.update", b24.Params{
    	"id": 5,
    	"fields": b24.Params{
    		"TITLE": "New Stage",
    		"COLOR": "FF5733",
    	},
    })
    if err != nil {
    	return fmt.Errorf("task.stages.update: %w", err)
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
        "start": 1793956087,
        "finish": 1793956087.677343,
        "duration": 0.6773428916931152,
        "processing": 0,
        "date_start": "2026-11-06T12:08:07+03:00",
        "date_finish": "2026-11-06T12:08:07+03:00"
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the stage was successfully updated
||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "You cannot manage stages"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `NOT_FOUND` | Stage was not found | There is no stage with the ID `id` ||
|| `400` | `ACCESS_DENIED` | You cannot manage stages | The user is not the owner or moderator of the group the stage belongs to, or the stage belongs to another user's "My Planner" ||
|| `400` | `EMPTY_TITLE` | Stage title is missing | An empty string is passed in `TITLE` ||
|| `400` | `DISPATCHER_ERROR` | Argument "id" is required, but no value passed for task.stages.update | The required parameter `id` is not passed. If `fields` is not passed, the error text contains its name ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-stages-add.md)
- [{#T}](./task-stages-get.md)
- [{#T}](./task-stages-delete.md)
- [{#T}](./task-stages-can-move-task.md)
- [{#T}](./task-stages-move-task.md)
