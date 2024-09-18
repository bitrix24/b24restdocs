# Get the stream tasks.flow.flow.get

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: stream team; user who can assign tasks to the stream

The method `tasks.flow.flow.get` returns the stream data by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **flowId*** [`integer`](../../data-types.md) | The identifier of the stream whose data needs to be retrieved.

You can obtain the identifier using the [tasks.task.get](../tasks-task-get.md) method for a task that has already been added to the stream, or create a new stream using the [tasks.flow.flow.create](./tasks-flow-flow-create.md) method ||
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
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.flow.flow.get
    ```

- cURL (oAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
    -d '{
        "flowId": 517
    }' \
    https://your-domain.bitrix24.com/rest/tasks.flow.flow.get
    ```

- JS

    ```js
    BX24.callMethod(
        'tasks.flow.flow.get',
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

- PHP

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    $flowId = 517;

    // executing the request to the REST API
    $result = CRest::call(
        'tasks.flow.flow.get',
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
        "efficiency": 100,
        "active": true,
        "plannedCompletionTime": 7200,
        "activity": "2024-08-22T12:17:48+00:00",
        "name": "Flow Name",
        "description": "Flow description",
        "distributionType": "manually",
        "responsibleQueue": [],
        "demo": false,
        "manualDistributorId": 3,
        "responsibleCanChangeDeadline": true,
        "matchWorkTime": true,
        "taskControl": false,
        "notifyAtHalfTime": false,
        "notifyOnQueueOverflow": null,
        "notifyOnTasksInProgressOverflow": 50,
        "notifyWhenEfficiencyDecreases": null,
        "taskCreators": [{"meta-user":  "all-users"}],
        "taskAssignees": [],
        "trialFeatureEnabled": false
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result** 
[`object`](../../data-types.md) | Object containing stream data ||
|| **id** 
[`integer`](../../data-types.md) | Stream identifier ||
|| **creatorId** 
[`integer`](../../data-types.md) | Identifier of the stream creator. Read-only ||
|| **ownerId** 
[`integer`](../../data-types.md) | Identifier of the stream administrator ||
|| **groupId** 
[`integer`](../../data-types.md) | Identifier of the group to which the stream is linked ||
|| **templateId** 
[`integer`](../../data-types.md) | Identifier of the template used to create tasks in the stream ||
|| **efficiency** 
[`integer`](../../data-types.md) | Efficiency of the stream in percentage. Read-only ||
|| **active** 
[`boolean`](../../data-types.md) | Status of the stream's activity ||
|| **plannedCompletionTime** 
[`integer`](../../data-types.md) | Planned time to complete the task in seconds ||
|| **activity** 
[`string`](../../data-types.md) | Date and time of the last activity in the stream. Read-only ||
|| **name** 
[`string`](../../data-types.md) | Name of the stream ||
|| **description** 
[`string`](../../data-types.md) | Description of the stream ||
|| **distributionType** 
[`string`](../../data-types.md) | Type of task distribution in the stream ||
|| **responsibleQueue** 
[`array`](../../data-types.md) | Stream team if the distribution type is queue. Empty when manually distributed ||
|| **manualDistributorId** 
[`integer`](../../data-types.md) | Identifier of the stream moderator when manually distributed (`null` when distributed by queue) ||
|| **demo** 
[`boolean`](../../data-types.md) | Indicates whether the stream is a demo. System parameter. Read-only ||
|| **responsibleCanChangeDeadline** 
[`boolean`](../../data-types.md) | Can the responsible person change the task deadline ||
|| **matchWorkTime** 
[`boolean`](../../data-types.md) | Should weekends and holidays be skipped when calculating the task deadline ||
|| **taskControl** 
[`boolean`](../../data-types.md) | Should the completed task be sent to the creator for review ||
|| **notifyAtHalfTime** 
[`boolean`](../../data-types.md) | Should the performer be notified at half the task deadline ||
|| **notifyOnQueueOverflow** 
[`integer`](../../data-types.md) | Number of tasks in the queue, exceeding which a notification will be sent to the stream administrator (if `null`, notifications are disabled) ||
|| **notifyOnTasksInProgressOverflow** 
[`integer`](../../data-types.md) | Number of tasks in progress, exceeding which a notification will be sent to the stream administrator (if `null`, notifications are disabled) ||
|| **notifyWhenEfficiencyDecreases** 
[`integer`](../../data-types.md) | Efficiency in percentage, below which a notification will be sent to the stream administrator (if `null`, notifications are disabled) ||
|| **taskCreators** 
[`object`](../../data-types.md) | List of users who can add tasks to the stream in the format `{"<object-type>": "<object-identifier>"}`, for example `[{"user": 3}, {"department": "17:F"}]`. The element `{"meta-user": "all-users"}` means that all users can add tasks ||
|| **taskAssignees** 
[`object`](../../data-types.md) | Participants of the project to which the stream is linked, if the distribution type is manual ||
|| **trialFeatureEnabled** 
[`boolean`](../../data-types.md) | Indicates whether the trial period is enabled for the stream. System parameter. Read-only ||
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
|| `0` | Access denied or flow not found | The portal plan may not allow working with streams or the user may not have permission to retrieve stream data ||
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