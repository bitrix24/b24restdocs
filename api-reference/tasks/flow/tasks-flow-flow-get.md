# Get tasks.flow.Flow.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: a user who can see the flow — a Bitrix24 administrator, a flow member, or an employee of a department specified in the flow. If all users can add tasks to the flow, all employees can see it

The method `tasks.flow.Flow.get` returns flow data by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **flowId***
[`integer`](../../data-types.md) | The identifier of the flow whose data needs to be retrieved.

The identifier is returned by the [tasks.flow.Flow.create](./tasks-flow-flow-create.md) method when a flow is created, and for a task from a flow — by the [tasks.task.get](../tasks-task-get.md) method ||
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
        "flowId": 517
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.flow.Flow.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "flowId": 517,
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.flow.Flow.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FlowGetResult = {
      id: number,
      creatorId: number,
      ownerId: number,
      groupId: number,
      templateId: number,
      efficiency: number,
      active: boolean,
      plannedCompletionTime: number,
      activity: ISODate | null,
      name: string,
      description: string,
      distributionType: string,
      responsibleList: string[][],
      demo: boolean,
      responsibleCanChangeDeadline: boolean,
      matchWorkTime: boolean,
      matchSchedule: boolean,
      taskControl: boolean,
      notifyAtHalfTime: boolean,
      notifyOnQueueOverflow: number | null,
      notifyOnTasksInProgressOverflow: number | null,
      notifyWhenEfficiencyDecreases: number | null,
      taskCreators: string[][],
      team: string[][],
      trialFeatureEnabled: boolean,
    }

    try {
      const response = await $b24.actions.v2.call.make<FlowGetResult>({
        method: 'tasks.flow.Flow.get',
        params: {
          flowId: 517,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Flow:', result.id, result.name, result.active)
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
      async function getFlow() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.flow.Flow.get',
            params: {
              flowId: 517,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Flow:', result.id, result.name, result.active)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getFlow)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.tasks.flow.flow.get(
            flow_id=517,
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
                'tasks.flow.Flow.get',
                [
                    'flowId' => 517
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        // The data processing logic you need
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting flow: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.flow.Flow.get',
        {
            flowId: 517
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
    require_once('crest.php'); // connecting the CRest PHP SDK

    $flowId = 517;

    // executing a request to the REST API
    $result = CRest::call(
        'tasks.flow.Flow.get',
        [
            'flowId' => $flowId
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
    res, err := client.Core().Call(ctx, "tasks.flow.Flow.get", b24.Params{
    	"flowId": 517,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("tasks.flow.Flow.get: %w", err)
    }

    var item struct {
    	ID         b24.ID `json:"id"`
    	CreatorID  b24.ID `json:"creatorId"`
    	OwnerID    b24.ID `json:"ownerId"`
    	GroupID    b24.ID `json:"groupId"`
    	TemplateID b24.ID `json:"templateId"`
    	Efficiency int    `json:"efficiency"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.CreatorID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "id": 517,
        "creatorId": 1,
        "ownerId": 1,
        "groupId": 6,
        "templateId": 0,
        "efficiency": 100,
        "active": true,
        "plannedCompletionTime": 7200,
        "activity": "2026-11-06T12:21:47+03:00",
        "name": "AFI flow 517",
        "description": "",
        "distributionType": "manually",
        "responsibleList": [
            [
                "user",
                "1"
            ]
        ],
        "demo": false,
        "responsibleCanChangeDeadline": true,
        "matchWorkTime": true,
        "matchSchedule": false,
        "taskControl": false,
        "notifyAtHalfTime": false,
        "notifyOnQueueOverflow": null,
        "notifyOnTasksInProgressOverflow": null,
        "notifyWhenEfficiencyDecreases": null,
        "taskCreators": [
            [
                "meta-user",
                "all-users"
            ]
        ],
        "team": [
            [
                "user",
                "129"
            ],
            [
                "user",
                "3"
            ]
        ],
        "trialFeatureEnabled": false
    },
    "time": {
        "start": 1793958310,
        "finish": 1793958310.539899,
        "duration": 0.5398991107940674,
        "processing": 0,
        "date_start": "2026-11-06T12:45:10+03:00",
        "date_finish": "2026-11-06T12:45:10+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object containing flow data [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Flow identifier ||
|| **creatorId**
[`integer`](../../data-types.md) | ID of the flow creator ||
|| **ownerId**
[`integer`](../../data-types.md) | ID of the flow administrator ||
|| **groupId**
[`integer`](../../data-types.md) | ID of the group to which the flow is linked ||
|| **templateId**
[`integer`](../../data-types.md) | ID of the template used to create tasks in the flow ||
|| **efficiency**
[`integer`](../../data-types.md) | Efficiency of the flow in percentage ||
|| **active**
[`boolean`](../../data-types.md) | Whether the flow is enabled ||
|| **plannedCompletionTime**
[`integer`](../../data-types.md) | How many seconds are allotted to complete a task, at least `3600` — one hour. The flow calculates the task deadline from this time ||
|| **activity**
[`datetime`](../../data-types.md) | Date and time of the last activity in the flow ||
|| **name**
[`string`](../../data-types.md) | Flow name ||
|| **description**
[`string`](../../data-types.md) | Description of the flow ||
|| **distributionType**
[`string`](../../data-types.md) | Type of task distribution in the flow ||
|| **responsibleList**
[`array`](../../data-types.md) | List of responsible persons for tasks in the flow; the items are "type — identifier" pairs. For manual distribution, this is the flow moderator ||
|| **demo**
[`boolean`](../../data-types.md) | Indicates whether the flow is a demo. System parameter ||
|| **responsibleCanChangeDeadline**
[`boolean`](../../data-types.md) | Can the responsible person change the task deadline ||
|| **matchWorkTime**
[`boolean`](../../data-types.md) | Whether to skip weekends and holidays when calculating the task deadline ||
|| **matchSchedule**
[`boolean`](../../data-types.md) | Whether to count the task completion time only during working hours according to the work schedule ||
|| **taskControl**
[`boolean`](../../data-types.md) | Whether to send the completed task to the creator for review ||
|| **notifyAtHalfTime**
[`boolean`](../../data-types.md) | Whether to notify the performer at half the task deadline ||
|| **notifyOnQueueOverflow**
[`integer`](../../data-types.md) | Number of tasks in the queue above which the flow administrator receives a notification. `null` — the notification is disabled ||
|| **notifyOnTasksInProgressOverflow**
[`integer`](../../data-types.md) | Number of tasks in progress above which the flow administrator receives a notification. `null` — the notification is disabled ||
|| **notifyWhenEfficiencyDecreases**
[`integer`](../../data-types.md) | Efficiency in percentage; when it drops below this value, the flow administrator receives a notification. `null` — the notification is disabled ||
|| **taskCreators**
[`array`](../../data-types.md) | Who can add tasks to the flow. Each item is a "type — identifier" pair, for example `["user", "3"]` or `["department", "17:F"]`. The pair `["meta-user", "all-users"]` means that all users can add tasks ||
|| **team**
[`array`](../../data-types.md) | Flow team.

For manual distribution, this includes all members of the group linked to the flow, except for the moderator.

For queue distribution and self-assignment, the team is the same as in `responsibleList`.

The items are "type — identifier" pairs, as in `taskCreators` ||
|| **trialFeatureEnabled**
[`boolean`](../../data-types.md) | Indicates whether the trial period is enabled for the flow. System parameter ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Flow with this ID does not exist."
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {flowId} | The required parameter `flowId` is missing ||
|| `400` | `0` | Cannot find flow | The user does not have access to the flow, or there is no flow with this `flowId` ||
|| `400` | `0` | Flow with this ID does not exist | There is no flow with this `flowId`. A Bitrix24 administrator receives this error, other users receive the access error ||
|| `400` | `0` | Unknown error. [Flow: 121] | Internal error. The number in brackets is the place in the code where the error occurred; it helps support find the cause ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-flow-flow-create.md)
- [{#T}](./tasks-flow-flow-update.md)
- [{#T}](./tasks-flow-flow-delete.md)
- [{#T}](./tasks-flow-flow-is-exists.md)
- [{#T}](./tasks-flow-flow-activate.md)
- [{#T}](./tasks-flow-flow-pin.md)
