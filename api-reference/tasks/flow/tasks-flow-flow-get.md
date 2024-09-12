# Get Flow tasks.flow.flow.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: flow team; user who can assign tasks in the flow

The method `tasks.flow.flow.get` returns flow data by its identifier.

## Method Parameters

#|
|| **Name** `type` | **Description** ||
|| **flowId^*^** [`integer`](../../data-types.md) | The identifier of the flow whose data needs to be retrieved. You can get the flowId using the [tasks.task.get](../tasks-task-get.md) method for a task that has already been added to the flow, or create a new flow using the [tasks.flow.flow.create](./tasks-flow-flow-create.md) method ||
|#

## Code Examples

{% list tabs %}

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

- cURL (Webhook)
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
        "flowId": 517
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.flow.flow.get
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

    // Handling the response from Bitrix24
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
|| **Name** `type` | **Description** ||
|| **result** [`object`](../../data-types.md) | Object containing flow data ||
|| **id** [`integer`](../../data-types.md) | Flow identifier ||
|| **creatorId** [`integer`](../../data-types.md) | Identifier of the flow creator. Read-only ||
|| **ownerId** [`integer`](../../data-types.md) | Identifier of the flow administrator ||
|| **groupId** [`integer`](../../data-types.md) | Identifier of the group to which the flow is linked ||
|| **templateId** [`integer`](../../data-types.md) | Identifier of the template used to create tasks in the flow ||
|| **efficiency** [`integer`](../../data-types.md) | Efficiency of the flow in percentage. Read-only ||
|| **active** [`boolean`](../../data-types.md) | Status of the flow's activity ||
|| **plannedCompletionTime** [`integer`](../../data-types.md) | Planned time to complete the task in seconds ||
|| **activity** [`string`](../../data-types.md) | Date and time of the last activity in the flow. Read-only ||
|| **name** [`string`](../../data-types.md) | Name of the flow ||
|| **description** [`string`](../../data-types.md) | Description of the flow ||
|| **distributionType** [`string`](../../data-types.md) | Type of task distribution in the flow ||
|| **responsibleQueue** [`array`](../../data-types.md) | Flow team, if the distribution type is queue. Empty for manual distribution ||
|| **manualDistributorId** [`integer`](../../data-types.md) | Identifier of the flow moderator for manual distribution (`null` for queue distribution) ||
|| **demo** [`boolean`](../../data-types.md) | Indicates whether the flow is a demo. System parameter. Read-only ||
|| **responsibleCanChangeDeadline** [`boolean`](../../data-types.md) | Can the responsible person change the task deadline ||
|| **matchWorkTime** [`boolean`](../../data-types.md) | Should weekends and holidays be skipped when calculating the task deadline ||
|| **taskControl** [`boolean`](../../data-types.md) | Should the completed task be sent to the Creator for review ||
|| **notifyAtHalfTime** [`boolean`](../../data-types.md) | Should the assignee be notified at half the task deadline ||
|| **notifyOnQueueOverflow** [`integer`](../../data-types.md) | Number of tasks in the queue, exceeding which will send a notification to the flow administrator (if `null`, notifications are disabled) ||
|| **notifyOnTasksInProgressOverflow** [`integer`](../../data-types.md) | Number of tasks in progress, exceeding which will send a notification to the flow administrator (if `null`, notifications are disabled) ||
|| **notifyWhenEfficiencyDecreases** [`integer`](../../data-types.md) | Efficiency percentage, below which a notification will be sent to the flow administrator (if `null`, notifications are disabled) ||
|| **taskCreators** [`object`](../../data-types.md) | List of users who can add tasks to the flow in the format {"<entity-type>": "<entity-identifier>"}, for example `[{"user": 3}, {"department": "17:F"}]`. The element `{"meta-user": "all-users"}` means that all users can add tasks ||
|| **taskAssignees** [`object`](../../data-types.md) | Participants of the project to which the flow is linked, if the distribution type is manual ||
|| **trialFeatureEnabled** [`boolean`](../../data-types.md) | Indicates whether the trial period is enabled for the flow. System parameter. Read-only ||
|#

## Error Handling

HTTP status: **200**

```json
{
    "error": "0",
    "error_description": "Flow not found"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** | **Additional Information** ||
|| `0` | Access denied or flow not found | The portal's plan may not allow working with flows, or the user may not have permission to retrieve flow data ||
|| `0` | Unknown error | Unknown error ||
|| `0` | Flow not found | Flow not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}