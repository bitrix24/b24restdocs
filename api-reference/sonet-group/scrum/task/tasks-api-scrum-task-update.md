# Create or Update a Scrum Task tasks.api.scrum.task.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with permission to modify the task and Scrum

The `tasks.api.scrum.task.update` method creates or updates a Scrum task. You can:
- create a task in Scrum
- transfer it between the backlog and sprints
- change story points
- link an epic

A task must be created using [tasks.task.add](../../../tasks/tasks-task-add.md) or updated using [tasks.task.update](../../../tasks/tasks-task-update.md). In the `GROUP_ID` field, specify the identifier of the same Scrum that contains the backlog or sprint from `entityId`. The `tasks.api.scrum.task.update` method does not move a task between projects.

You can obtain the group identifier using the [create new group](../../sonet-group-create.md) method or the [get group list](../../socialnetwork-api-workgroup-list.md) method. A group is considered Scrum if the `SCRUM_MASTER_ID` field is filled.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Task identifier ||
|| **fields***
[`object`](../../../data-types.md) | An object containing records about the Scrum task (detailed description provided [below](#parameter-fields)) in the following structure:

```js
fields: {
    entityId: 'value',
    storyPoints: 'value',
    epicId: 'value',
    sort: 'value',
    sortFloat: 'value',
    createdBy: 'value',
    modifiedBy: 'value'
}
```

||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **entityId**
`integer` | Identifier of the backlog or sprint.

The parameter is required when adding a task to Scrum. For an existing Scrum task, you can omit it if you do not need to move the task between the backlog and sprints ||
|| **storyPoints**
`string` | Story Points — a relative estimate of the task's complexity.

Can have a string value ||
|| **epicId**
`integer` | Epic identifier ||
|| **sort**
`integer` | Sorting ||
|| **sortFloat**
`float` | Sorting value with a fractional part ||
|| **createdBy**
`integer` | Identifier of the user who created the Scrum task record ||
|| **modifiedBy**
`integer` | Identifier of the user who modified the Scrum task record ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"fields":{"epicId":1,"storyPoints":"8","entityId":2}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.api.scrum.task.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"fields":{"epicId":1,"storyPoints":"8","entityId":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.api.scrum.task.update
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
        method: 'tasks.api.scrum.task.update',
        params: {
          id: 1,
          fields: {
            epicId: 1,
            storyPoints: '8',
            entityId: 2,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Scrum task updated:', result)
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
      async function updateScrumTask() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.api.scrum.task.update',
            params: {
              id: 1,
              fields: {
                epicId: 1,
                storyPoints: '8',
                entityId: 2,
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
          console.info('Scrum task updated:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateScrumTask)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.tasks.api.scrum.task.update(
            bitrix_id=1,
            fields={
                "epicId": 1,
                "storyPoints": "8",
                "entityId": 2,
            },
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
                'tasks.api.scrum.task.update',
                [
                    'id' => 1,
                    'fields' => [
                        'epicId'      => 1,
                        'storyPoints' => '8',
                        'entityId'    => 2
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating scrum task: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.api.scrum.task.update',
        {
            id: 1,
            fields: 
            {
                epicId: 1,
                storyPoints: '8',
                entityId: 2
            }
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.api.scrum.task.update',
        [
            'id' => 1,
            'fields' => [
                'epicId' => 1,
                'storyPoints' => '8',
                'entityId' => 2
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "tasks.api.scrum.task.update", b24.Params{
    	"id": 1,
    	"fields": b24.Params{
    		"epicId":      1,
    		"storyPoints": "8",
    		"entityId":    2,
    	},
    })
    if err != nil {
    	return fmt.Errorf("tasks.api.scrum.task.update: %w", err)
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
    "result": true,
    "time": {
        "start": 1721402687.900315,
        "finish": 1721402694.313811,
        "duration": 6.413496017456055,
        "processing": 6.387248992919922,
        "date_start": "2024-07-19T15:24:47+00:00",
        "date_finish": "2024-07-19T15:24:54+00:00",
        "operating": 6.387217998504639
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Returns `true` if the Scrum task was created or updated ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description": "Task not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Task id not found | The task identifier is `0` ||
|| `0` | Entity id not found | `entityId` was not passed when adding the task to Scrum ||
|| `0` | Entity not found. | The backlog or sprint with the specified `entityId` was not found ||
|| `0` | Epic not found | Epic not found ||
|| `0` | Task not found. | Task not found ||
|| `0` | Task not found. The task must be in the project of the entity | The task and the backlog or sprint from `entityId` belong to different projects ||
|| `0` | Access denied | Access denied ||
|| `0` | Item not created | Task not added to Scrum ||
|| `0` | createdBy user not found | The user specified in `createdBy` was not found ||
|| `0` | modifiedBy user not found | The user specified in `modifiedBy` was not found ||
|| `0` | Unable to update task | Failed to save changes to the Scrum task ||
|| `100` | Could not find value for parameter {id} | The required `id` parameter is missing ||
|| `100` | Could not find value for parameter {fields} | The required `fields` parameter is missing ||
|| `100` | Invalid value {stringValue} to match with parameter {id}. Should be value of type int. | The `id` parameter has an invalid type ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-task-get.md)
- [{#T}](./tasks-api-scrum-task-get-fields.md)
