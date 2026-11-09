# Add a Kanban or "My Planner" Stage task.stages.add

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

The method `task.stages.add` adds a Kanban or "My Planner" stage.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for adding a new stage [(detailed description)](#fields) ||
|| **isAdmin**
[`boolean`](../../data-types.md) | If `true` is passed, the method does not check permissions in the group. The parameter applies only to a Bitrix24 administrator; for other users, permissions are checked as usual. Default is `false` ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE***
[`string`](../../data-types.md) | Stage name ||
|| **COLOR**
[`string`](../../data-types.md) | Stage color — a 6-character hexadecimal RGB code without `#`, for example `FFAAEE`.

The method does not validate the format. If you pass `#FFAAEE`, the truncated value `#FFAAE` is retained ||
|| **AFTER_ID**
[`integer`](../../data-types.md) | Identifier of the stage after which the new stage should be added.

If not passed or `0` is passed, the stage becomes the first one. If there is no stage with this ID, the stage becomes the last one ||
|| **ENTITY_ID**
[`integer`](../../data-types.md) | Where to create the stage:
- group `ID` — in the group's Kanban
- `0` or not passed — in "My Planner" of the current user

If the user is not the group owner or moderator, the method returns an access error ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
    "fields": {
        "TITLE": "Stage title",
        "COLOR": "FFAAEE",
        "AFTER_ID": 1,
        "ENTITY_ID": 1
    },
    "isAdmin": false
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.stages.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
    "fields": {
        "TITLE": "Stage title",
        "COLOR": "FFAAEE",
        "AFTER_ID": 1,
        "ENTITY_ID": 1
    },
    "isAdmin": false,
    "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/task.stages.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // ID of the added stage returned in result
    // Shape of the payload returned in result (match the "response handling" section of the page)
    type StageAddResult = number

    try {
      const response = await $b24.actions.v2.call.make<StageAddResult>({
        method: 'task.stages.add',
        params: {
          fields: {
            TITLE: 'Stage title',
            COLOR: 'FFAAEE',
            AFTER_ID: 1,
            ENTITY_ID: 1,
          },
          isAdmin: false,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('New stage ID:', result)
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
      async function addTaskStage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.stages.add',
            params: {
              fields: {
                TITLE: 'Stage title',
                COLOR: 'FFAAEE',
                AFTER_ID: 1,
                ENTITY_ID: 1,
              },
              isAdmin: false,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('New stage ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addTaskStage)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "TITLE": "Stage name",
        "COLOR": "FFAAEE",
        "AFTER_ID": 1,
        "ENTITY_ID": 1,
    }

    try:
        bitrix_response = client.task.stages.add(
            fields=fields,
            is_admin=False,
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
                'task.stages.add',
                [
                    'fields' => [
                        'TITLE'    => 'Stage title',
                        'COLOR'    => 'FFAAEE',
                        'AFTER_ID' => 1,
                        'ENTITY_ID' => 1
                    ],
                    'isAdmin' => false,
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
        echo 'Error adding task stage: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.stages.add',
        {
            fields: {
                TITLE: 'Stage title',
                COLOR: 'FFAAEE',
                AFTER_ID: 1,
                ENTITY_ID: 1
            },
            isAdmin: false,
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
    require_once('crest.php'); // include CRest PHP SDK

    $fields = [
        "TITLE" => "Stage title",
        "COLOR" => "FFAAEE",
        "AFTER_ID" => 1,
        "ENTITY_ID" => 1
    ];

    // execute request to REST API
    $result = CRest::call(
        'task.stages.add',
        [
            'fields' => $fields,
            'isAdmin' => false
        ]
    );

    // Process the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "task.stages.add", b24.Params{
    	"fields": b24.Params{
    		"TITLE":     "Stage title",
    		"COLOR":     "FFAAEE",
    		"AFTER_ID":  1,
    		"ENTITY_ID": 1,
    	},
    	"isAdmin": false,
    })
    if err != nil {
    	return fmt.Errorf("task.stages.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 1,
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
[`integer`](../../data-types.md) | Identifier of the added stage ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "EMPTY_TITLE",
    "error_description": "Stage title is missing"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `EMPTY_TITLE` | Stage title is missing | `TITLE` is not passed or an empty string is passed ||
|| `400` | `ACCESS_DENIED` | You cannot manage stages | The user is not the owner or moderator of the group from `ENTITY_ID`, or there is no group with this ID ||
|| `400` | `DISPATCHER_ERROR` | Argument "fields" is required, but no value passed for task.stages.add | The required parameter `fields` is not passed ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-stages-update.md)
- [{#T}](./task-stages-get.md)
- [{#T}](./task-stages-delete.md)
- [{#T}](./task-stages-can-move-task.md)
- [{#T}](./task-stages-move-task.md)
