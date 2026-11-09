# Update the Flow tasks.flow.Flow.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: Bitrix24 administrator, flow creator, or flow administrator

The method `tasks.flow.Flow.update` modifies the flow.

{% note warning "" %}

The method resets settings that are not passed in `flowData` to their default values:
- description `description`
- flags `matchWorkTime`, `matchSchedule`, `responsibleCanChangeDeadline`, `notifyAtHalfTime`, `taskControl`
- notifications `notifyOnQueueOverflow`, `notifyOnTasksInProgressOverflow`, `notifyWhenEfficiencyDecreases`

For example, a call with only `id` and `name` renames the flow, clears its description, and turns off notifications.

To change one field without losing the others, retrieve the flow with the [tasks.flow.Flow.get](./tasks-flow-flow-get.md) method and pass the current values of these settings along with the change.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **flowData***
[`object`](../../data-types.md) | Field values to modify the flow [(detailed description)](#flowdata) ||
|#

### Parameter flowData {#flowdata}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the flow to be modified.

The ID is returned by the [tasks.flow.Flow.create](./tasks-flow-flow-create.md) method when the flow is created, and for a task from the flow, by the [tasks.task.get](../tasks-task-get.md) method ||
|| **name**
[`string`](../../data-types.md) | The name of the flow, from 1 to 255 characters. Must be unique within Bitrix24.

You can check the name using the method [tasks.flow.Flow.isExists](./tasks-flow-flow-is-exists.md) ||
|| **description**
[`string`](../../data-types.md) | Description of the flow. If not passed, the description becomes empty ||
|| **groupId**
[`integer`](../../data-types.md) | The ID of the group to which the flow will be linked. If not passed, the group does not change ||
|| **ownerId**
[`integer`](../../data-types.md) | The ID of the flow administrator. If not passed, the administrator does not change ||
|| **templateId**
[`integer`](../../data-types.md) | The ID of the template that users will use to add tasks to the flow ||
|| **plannedCompletionTime**
[`integer`](../../data-types.md) | Number of seconds allowed to complete a task, at least `3600` (one hour). The flow calculates the task deadline from this time. If not passed, the time does not change ||
|| **distributionType**
[`string`](../../data-types.md) | Type of distribution:
- `manually` — manual distribution
- `queue` — distribution by queue
- `himself` — self-distribution

More about distribution types can be found in the article [{#T}](./index.md).

If not passed, the distribution type does not change ||
|| **responsibleList**
[`array`](../../data-types.md) | Employees or departments that will receive tasks. Each element is a "type — ID" pair.

For manual distribution, specify a single pair with the flow moderator, for example `["user", "1"]`.

For self-distribution or queue distribution, specify employees or departments. For example:

```js
[
    [
        'department','3'
    ],
    [
        'department','17:F'
    ]
]
```

A department without the `:F` suffix is included together with all its sub-departments according to the company structure. With the `:F` suffix, only the department itself is included.

If not passed, the list of responsible persons does not change ||
|| **taskCreators**
[`array`](../../data-types.md) | Who can add tasks to the flow: users or departments. Each element is a `["<object-type>", "<object-id>"]` pair, for example:

```js
[
    [
        'user','3'
    ],
    [
        'department','17:F'
    ]
]
```

A department without the `:F` suffix is included together with all its sub-departments according to the company structure. With the `:F` suffix, only the department itself is included

To allow all users to add tasks, pass the pair `["meta-user", "all-users"]`.

If not passed, the list does not change ||
|| **matchWorkTime**
[`integer`](../../data-types.md) | Skip weekends and holidays when calculating the task deadline.

Accepts values `0` and `1`. If not passed, `1` is used ||
|| **matchSchedule**
[`integer`](../../data-types.md) | Count the task completion time only during working hours according to the work schedule.

Accepts values `0` and `1`. If not passed, `0` is used ||
|| **responsibleCanChangeDeadline**
[`integer`](../../data-types.md) | Can the responsible person change the task deadline.

Accepts values `0` and `1`. If not passed, `1` is used ||
|| **notifyAtHalfTime**
[`integer`](../../data-types.md) | Notify the performer at half the task deadline.

Accepts values `0` and `1`. If not passed, `0` is used ||
|| **taskControl**
[`integer`](../../data-types.md) | Send the completed task to the creator for review.

Accepts values `0` and `1`. If not passed, `0` is used ||
|| **notifyOnQueueOverflow**
[`integer`](../../data-types.md) | Number of tasks in the queue above which the flow administrator receives a notification.

If not passed, the notification is turned off ||
|| **notifyOnTasksInProgressOverflow**
[`integer`](../../data-types.md) | Number of tasks in progress above which the flow administrator receives a notification.

If not passed, the notification is turned off ||
|| **notifyWhenEfficiencyDecreases**
[`integer`](../../data-types.md) | Efficiency percentage below which the flow administrator receives a notification.

If not passed, the notification is turned off ||
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
        "flowData": {
            "id": 517,
            "name": "Updated Flow Name",
            "description": "Updated description",
            "plannedCompletionTime": 7200,
            "distributionType": "manually",
            "responsibleList": [["user","3"]],
            "taskCreators": [["meta-user","all-users"]],
            "matchWorkTime": 1,
            "notifyAtHalfTime": 0
        }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.flow.Flow.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "flowData": {
            "id": 517,
            "name": "Updated Flow Name",
            "description": "Updated description",
            "plannedCompletionTime": 7200,
            "distributionType": "manually",
            "responsibleList": [["user","3"]],
            "taskCreators": [["meta-user","all-users"]],
            "matchWorkTime": 1,
            "notifyAtHalfTime": 0
        },
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.flow.Flow.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type FlowUpdateResult = {
      id: number
      creatorId: number
      ownerId: number
      groupId: number
      templateId: number
      efficiency: number
      active: boolean
      plannedCompletionTime: number
      activity: ISODate
      name: string
      description: string
      distributionType: string
      responsibleList: string[][]
      demo: boolean
      responsibleCanChangeDeadline: boolean
      matchWorkTime: boolean
      matchSchedule: boolean
      taskControl: boolean
      notifyAtHalfTime: boolean
      notifyOnQueueOverflow: number | null
      notifyOnTasksInProgressOverflow: number | null
      notifyWhenEfficiencyDecreases: number | null
      taskCreators: string[][]
      team: string[][]
      trialFeatureEnabled: boolean
    }

    try {
      const response = await $b24.actions.v2.call.make<FlowUpdateResult>({
        method: 'tasks.flow.Flow.update',
        params: {
          flowData: {
            id: 517,
            name: 'Updated Flow Name',
            description: 'Updated description',
            plannedCompletionTime: 7200,
            distributionType: 'manually',
            responsibleList: [
              [
                'user', '3',
              ],
            ],
            taskCreators: [
              [
                'meta-user', 'all-users',
              ],
            ],
            matchWorkTime: 1,
            notifyAtHalfTime: 0,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated flow:', result.id, result.name, result.distributionType)
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
      async function updateFlow() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.flow.Flow.update',
            params: {
              flowData: {
                id: 517,
                name: 'Updated Flow Name',
                description: 'Updated description',
                plannedCompletionTime: 7200,
                distributionType: 'manually',
                responsibleList: [
                  [
                    'user', '3',
                  ],
                ],
                taskCreators: [
                  [
                    'meta-user', 'all-users',
                  ],
                ],
                matchWorkTime: 1,
                notifyAtHalfTime: 0,
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
          console.info('Updated flow:', result.id, result.name, result.distributionType)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateFlow)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    flow_data = {
        "id": 517,
        "name": "Updated Flow Name",
        "description": "Updated description",
        "plannedCompletionTime": 7200,
        "distributionType": "manually",
        "responsibleList": [
            [
                "user",
                "3",
            ],
        ],
        "taskCreators": [
            [
                "meta-user",
                "all-users",
            ],
        ],
        "matchWorkTime": 1,
        "notifyAtHalfTime": 0,
    }

    try:
        bitrix_response = client.tasks.flow.flow.update(
            flow_data=flow_data,
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
                'tasks.flow.Flow.update',
                [
                    'flowData' => [
                        'id'                   => 517,
                        'name'                 => 'Updated Flow Name',
                        'description'          => 'Updated description',
                        'plannedCompletionTime' => 7200,
                        'distributionType'     => 'manually',
                        'responsibleList'      => [
                            ['user', '3']
                        ],
                        'taskCreators'         => [
                            ['meta-user', 'all-users']
                        ],
                        'matchWorkTime'        => 1,
                        'notifyAtHalfTime'     => 0
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating flow: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.flow.Flow.update',
        {
            flowData: {
                id: 517,
                name: 'Updated Flow Name',
                description: 'Updated description',
                plannedCompletionTime: 7200,
                distributionType: 'manually',
                responsibleList: [
                    [
                        'user','3'
                    ]
                ],
                taskCreators: [
                    [
                        'meta-user','all-users'
                    ]
                ],
                matchWorkTime: 1,
                notifyAtHalfTime: 0
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
    require_once('crest.php'); // connecting the CRest PHP SDK

    $flowData = [
        "id" => 517,
        "name" => "Updated Flow Name",
        "description" => "Updated description",
        "plannedCompletionTime" => 7200,
        "distributionType" => "manually",
        "responsibleList" => [["user", "3"]],
        "taskCreators" => [["meta-user", "all-users"]],
        "matchWorkTime" => 1,
        "notifyAtHalfTime" => 0
    ];

    // executing a request to the REST API
    $result = CRest::call(
        'tasks.flow.Flow.update',
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "tasks.flow.Flow.update", b24.Params{
    	"flowData": b24.Params{
    		"id":                    517,
    		"name":                  "Updated Flow Name",
    		"description":           "Updated description",
    		"plannedCompletionTime": 7200,
    		"distributionType":      "manually",
    		"responsibleList": []any{
    			[]string{"user", "3"},
    		},
    		"taskCreators": []any{
    			[]string{"meta-user", "all-users"},
    		},
    		"matchWorkTime":    1,
    		"notifyAtHalfTime": 0,
    	},
    })
    if err != nil {
    	return fmt.Errorf("tasks.flow.Flow.update: %w", err)
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
        "groupId": 7,
        "templateId": 0,
        "efficiency": 100,
        "active": true,
        "plannedCompletionTime": 7200,
        "activity": "2026-11-06T12:06:34+03:00",
        "name": "Updated Flow Name",
        "description": "Updated description",
        "distributionType": "manually",
        "responsibleList": [
            [
                "user",
                "3"
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
        "taskCreators": [],
        "team": [],
        "trialFeatureEnabled": false
    },
    "time": {
        "start": 1793956397,
        "finish": 1793956397.557803,
        "duration": 0.5578029155731201,
        "processing": 0,
        "date_start": "2026-11-06T12:13:17+03:00",
        "date_finish": "2026-11-06T12:13:17+03:00"
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
[`integer`](../../data-types.md) | ID of the updated flow ||
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
[`integer`](../../data-types.md) | Number of seconds allowed to complete a task, at least `3600` (one hour). The flow calculates the task deadline from this time ||
|| **activity**
[`datetime`](../../data-types.md) | Date and time of the last activity in the flow ||
|| **name**
[`string`](../../data-types.md) | Flow name ||
|| **description**
[`string`](../../data-types.md) | Description of the flow ||
|| **distributionType**
[`string`](../../data-types.md) | Type of task distribution in the flow ||
|| **responsibleList**
[`array`](../../data-types.md) | List of responsible persons for tasks in the flow, with "type — ID" pairs as elements. For manual distribution, this is the flow moderator ||
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
[`integer`](../../data-types.md) | Efficiency percentage below which the flow administrator receives a notification. `null` — the notification is disabled ||
|| **taskCreators**
[`array`](../../data-types.md) | Who can add tasks to the flow. Each element is a "type — ID" pair, for example `["user", "3"]` or `["department", "17:F"]`. The pair `["meta-user", "all-users"]` means that all users can add tasks.

In the response of this method, the field is empty even if task creators are set. The current list is returned by the [tasks.flow.Flow.get](./tasks-flow-flow-get.md) method ||
|| **team**
[`array`](../../data-types.md) | Flow team.

For manual distribution, this includes all members of the group linked to the flow, except for the moderator.

For queue and self-distribution, the team is the same as in `responsibleList`.

Elements are "type — ID" pairs, as in `taskCreators`. In the response of this method, the field is empty; the current team is returned by the [tasks.flow.Flow.get](./tasks-flow-flow-get.md) method ||
|| **trialFeatureEnabled**
[`boolean`](../../data-types.md) | Indicates whether the trial period is enabled for the flow. System parameter ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "'id': field's value must be greater than 0"
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | 'id': field's value must be greater than 0 | `flowData.id` is not passed or the value is less than `1` ||
|| `400` | `0` | Cannot find flow | The user does not have the permission to modify the flow, or a flow with this `id` does not exist ||
|| `400` | `0` | 'plannedCompletionTime': field's value must be greater than 3600 | The completion time is less than an hour: `plannedCompletionTime` is less than `3600` seconds ||
|| `400` | `0` | distributionType: unexpected value | `distributionType` contains a value that is not in the list ||
|| `400` | `0` | A flow with this name already exists | The flow name is already taken in Bitrix24. You can check the name in advance using the method [tasks.flow.Flow.isExists](./tasks-flow-flow-is-exists.md) ||
|| `400` | `0` | Unknown error. [Flow: 267] | A flow with this `id` does not exist. Bitrix24 administrators receive this error, other users receive the access error ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-flow-flow-create.md)
- [{#T}](./tasks-flow-flow-get.md)
- [{#T}](./tasks-flow-flow-delete.md)
- [{#T}](./tasks-flow-flow-is-exists.md)
- [{#T}](./tasks-flow-flow-activate.md)
- [{#T}](./tasks-flow-flow-pin.md)
