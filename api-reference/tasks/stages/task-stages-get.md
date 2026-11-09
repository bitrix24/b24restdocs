# Get the List of Kanban Stages or "My Planner" task.stages.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user for stages of their own "My Planner"
> - a user with permission to view the group's tasks for the group's Kanban stages

The method `task.stages.get` retrieves Kanban stages or "My Planner" stages. If there are no stages yet, the method first creates the default stages.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **entityId*** 
[`integer`](../../data-types.md) | Whose stages to retrieve:
- group `ID` — the group's Kanban stages. If the user does not have permission to view the group's tasks, the method returns an access error
- `0` — the "My Planner" stages of the current user ||
|| **numeric**
[`boolean`](../../data-types.md) | If `true` is passed, the method returns the stages as an array rather than an object keyed by stage IDs. Default is `false` ||
|| **isAdmin** 
[`boolean`](../../data-types.md) | If `true` is passed, the method does not check permissions in the group. The parameter applies only to a Bitrix24 administrator; for other users, permissions are checked as usual. Default is `false` ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
    "entityId": 0
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.stages.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
    "entityId": 0,
    "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/task.stages.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each stage in the result map
    type StageInfo = {
      ID: string
      TITLE: string
      SORT: string
      COLOR: string
      SYSTEM_TYPE: string | null
      ENTITY_ID: string
      ENTITY_TYPE: string
      ADDITIONAL_FILTER: unknown[]
      TO_UPDATE: unknown[]
      TO_UPDATE_ACCESS: null
      CAN_SORT_ITEMS?: boolean
    }

    // result is a map of stage ID → StageInfo
    // Shape of the payload returned in result (match the "response handling" section of the page)
    type StagesGetResult = Record<string, StageInfo>

    try {
      const response = await $b24.actions.v2.call.make<StagesGetResult>({
        method: 'task.stages.get',
        params: {
          entityId: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Stages:', Object.values(result).map(s => `${s.ID}: ${s.TITLE}`))
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
      async function getTaskStages() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.stages.get',
            params: {
              entityId: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Stages:', Object.values(result).map(s => `${s.ID}: ${s.TITLE}`))
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTaskStages)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.task.stages.get(
            entity_id=0,
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
        $entityId = 0;
        $response = $b24Service
            ->core
            ->call(
                'task.stages.get',
                [
                    'entityId' => $entityId,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting task stages: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const entityId = 0;
    BX24.callMethod(
        'task.stages.get',
        {
            entityId: entityId,
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

    $entityId = 0;

    // executing the request to the REST API
    $result = CRest::call(
        'task.stages.get',
        [
            'entityId' => $entityId
        ]
    );

    // Processing the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "task.stages.get", b24.Params{
    	"entityId": 0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("task.stages.get: %w", err)
    }

    keys, ok := b24.Keys(res.Result)
    if !ok {
    	return fmt.Errorf("expected an object in the response")
    }
    fmt.Println("fields in response:", len(keys))
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "19": {
            "ID": "19",
            "TITLE": "Not planned",
            "SORT": "100",
            "COLOR": "00C4FB",
            "SYSTEM_TYPE": "NEW",
            "ENTITY_ID": "129",
            "ENTITY_TYPE": "U",
            "ADDITIONAL_FILTER": [],
            "TO_UPDATE": [],
            "TO_UPDATE_ACCESS": null,
            "CAN_SORT_ITEMS": true
        },
        "20": {
            "ID": "20",
            "TITLE": "In progress",
            "SORT": "200",
            "COLOR": "9DCF00",
            "SYSTEM_TYPE": null,
            "ENTITY_ID": "129",
            "ENTITY_TYPE": "U",
            "ADDITIONAL_FILTER": [],
            "TO_UPDATE": [],
            "TO_UPDATE_ACCESS": null
        }
    },
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
[`object`](../../data-types.md) | Kanban or "My Planner" stages. The key is the stage ID, the value is the stage object [(detailed description)](#stage).

If you pass `numeric: true`, an array of stage objects is returned ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Stage Object {#stage}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Stage ID ||
|| **TITLE**
[`string`](../../data-types.md) | Stage name ||
|| **SORT**
[`string`](../../data-types.md) | Stage sequence number: the smaller the number, the further left the stage is in the Kanban ||
|| **COLOR**
[`string`](../../data-types.md) | Stage color — a hexadecimal RGB code without `#` ||
|| **SYSTEM_TYPE**
[`string`](../../data-types.md) | System type of the stage: `NEW`, `WORK`, `REVIEW`, or `FINISH`. A stage with a system type cannot be deleted. The first stage always has `NEW`. A regular stage has `null` or an empty string ||
|| **ENTITY_ID**
[`string`](../../data-types.md) | ID of the group or user that owns the stages ||
|| **ENTITY_TYPE**
[`string`](../../data-types.md) | Type of the stage owner: `U` — the user's "My Planner", `G` — the group's Kanban ||
|| **ADDITIONAL_FILTER**
[`array`](../../data-types.md) | System parameter, always an empty array ||
|| **TO_UPDATE**
[`array`](../../data-types.md) | System parameter, always an empty array ||
|| **TO_UPDATE_ACCESS**
[`null`](../../data-types.md) | System parameter, always `null` ||
|| **CAN_SORT_ITEMS**
[`boolean`](../../data-types.md) | Whether tasks can be sorted within the stage. Returned only for stages with a system type ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "You cannot view tasks in this group"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ACCESS_DENIED` | You cannot view tasks in this group | The user does not have permission to view tasks of the group from `entityId` ||
|| `400` | `DISPATCHER_ERROR` | Argument "entityid" is required, but no value passed for task.stages.get | The required parameter `entityId` is not passed ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-stages-add.md)
- [{#T}](./task-stages-update.md)
- [{#T}](./task-stages-delete.md)
- [{#T}](./task-stages-can-move-task.md)
- [{#T}](./task-stages-move-task.md)
