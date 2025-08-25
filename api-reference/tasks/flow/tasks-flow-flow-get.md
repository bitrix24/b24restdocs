# Get Flow tasks.flow.Flow.get

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: flow team; user who can assign tasks in the flow

The method `tasks.flow.Flow.get` returns flow data by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **flowId*** [`integer`](../../data-types.md) | The identifier of the flow whose data needs to be retrieved.

You can obtain the identifier by creating a new flow using the method [tasks.flow.Flow.create](./tasks-flow-flow-create.md) or by retrieving a task using the method [tasks.task.get](../tasks-task-get.md) for a task from the flow ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
        "flowId": 517
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.flow.Flow.get
    ```

- cURL (oAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
    -d '{
        "flowId": 517
    }' \
    https://your-domain.bitrix24.com/rest/tasks.flow.Flow.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'tasks.flow.Flow.get',
    		{
    			flowId: 517
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
    {
    	console.error(error);
    }
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
        // Your logic for processing data
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
    require_once('crest.php'); // connecting CRest PHP SDK

    $flowId = 517;

    // executing request to REST API
    $result = CRest::call(
        'tasks.flow.Flow.get',
        [
            'flowId' => $flowId
        ]
    );

    // Processing response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
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
        "groupId": 178,
        "templateId": 0,
        "efficiency": 0,
        "active": true,
        "plannedCompletionTime": 7200,
        "activity": "2024-09-02T15:27:29+00:00",
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
        "taskControl": false,
        "notifyAtHalfTime": false,
        "notifyOnQueueOverflow": 10,
        "notifyOnTasksInProgressOverflow": 50,
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
                "3"
            ]
        ],
        "trialFeatureEnabled": false
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result** 
[`object`](../../data-types.md) | Object with flow data ||
|| **id** 
[`integer`](../../data-types.md) | Flow identifier ||
|| **creatorId** 
[`integer`](../../data-types.md) | Identifier of the flow creator. Read-only ||
|| **ownerId** 
[`integer`](../../data-types.md) | Identifier of the flow administrator ||
|| **groupId** 
[`integer`](../../data-types.md) | Identifier of the group to which the flow is linked ||
|| **templateId** 
[`integer`](../../data-types.md) | Identifier of the template used to create tasks in the flow ||
|| **efficiency** 
[`integer`](../../data-types.md) | Flow efficiency in percentage. Read-only ||
|| **active** 
[`boolean`](../../data-types.md) | Flow active status ||
|| **plannedCompletionTime** 
[`integer`](../../data-types.md) | Planned task completion time in seconds ||
|| **activity** 
[`string`](../../data-types.md) | Date and time of the last activity in the flow. Read-only ||
|| **name** 
[`string`](../../data-types.md) | Flow name ||
|| **description** 
[`string`](../../data-types.md) | Flow description ||
|| **distributionType** 
[`string`](../../data-types.md) | Type of task distribution in the flow ||
|| **responsibleList**
[`array`](../../data-types.md) | List of responsible persons for tasks in the flow. For manual distribution, this is the flow moderator ||
|| **demo** 
[`boolean`](../../data-types.md) | Indicates if the flow is a demo. System parameter. Read-only ||
|| **responsibleCanChangeDeadline** 
[`boolean`](../../data-types.md) | Can the responsible person change the task deadline ||
|| **matchWorkTime** 
[`boolean`](../../data-types.md) | Should weekends and holidays be skipped when calculating the task deadline ||
|| **taskControl** 
[`boolean`](../../data-types.md) | Should the completed task be sent to the creator for review ||
|| **notifyAtHalfTime** 
[`boolean`](../../data-types.md) | Should the assignee be notified at half the task deadline ||
|| **notifyOnQueueOverflow** 
[`integer`](../../data-types.md) | Number of tasks in the queue, exceeding which a notification will be sent to the flow administrator (if `null`, notifications are disabled) ||
|| **notifyOnTasksInProgressOverflow** 
[`integer`](../../data-types.md) | Number of tasks in progress, exceeding which a notification will be sent to the flow administrator (if `null`, notifications are disabled) ||
|| **notifyWhenEfficiencyDecreases** 
[`integer`](../../data-types.md) | Efficiency in percentage, below which a notification will be sent to the flow administrator (if `null`, notifications are disabled) ||
|| **taskCreators** 
[`object`](../../data-types.md) | List of users who can add tasks to the flow in the format `{"<object-type>": "<object-identifier>"}`. For example, `[{"user": 3}, {"department": "17:F"}]`.

The element `{"meta-user": "all-users"}` means that all users can add tasks ||
|| **team**
[`object`](../../data-types.md) | Flow team.

For manual distribution, this includes all project participants linked to the flow, except for the moderator. 

For queue distribution and self-distribution, the team is the same as in `responsibleList` ||
|| **trialFeatureEnabled** 
[`boolean`](../../data-types.md) | Indicates if the trial period is enabled for the flow. System parameter. Read-only ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Flow not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Additional Information** ||
|| `0` | Access denied or flow not found | The account plan does not allow working with flows or the user does not have permission to retrieve flow data ||
|| `0` | `Unknown error` | Unknown error ||
|| `0` | `Flow not found` | Flow not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-flow-flow-create.md)
- [{#T}](./tasks-flow-flow-update.md)
- [{#T}](./tasks-flow-flow-delete.md)
- [{#T}](./tasks-flow-flow-is-exists.md)
- [{#T}](./tasks-flow-flow-activate.md)
- [{#T}](./tasks-flow-flow-pin.md)