# Update the flow tasks.flow.flow.update

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: the Creator or the administrator of the flow

The method `tasks.flow.flow.update` modifies the flow.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **flowData*** 
[`object`](../../data-types.md) | Field values for modifying the flow (detailed description provided below) ||
|#

### Parameter flowData

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../data-types.md) | Identifier of the flow to be modified. 

You can obtain the identifier using the method [tasks.task.get](../tasks-task-get.md) for a task already added to the flow, or create a new flow using the method [tasks.flow.flow.create](./tasks-flow-flow-create.md) ||
|| **name** 
[`string`](../../data-types.md) | Name of the flow. Must be unique for each flow. 

To check the name, you can use the method [tasks.flow.flow.isExists](./tasks-flow-flow-is-exists.md) ||
|| **description** 
[`string`](../../data-types.md) | Description of the flow ||
|| **groupId** 
[`integer`](../../data-types.md) | Identifier of the group to which the flow will be linked. 

If not specified, a new group is automatically created ||
|| **ownerId** 
[`integer`](../../data-types.md) | Identifier of the flow administrator. 

If not specified, the creator of the flow will be the administrator ||
|| **templateId** 
[`integer`](../../data-types.md) | Identifier of the template that users will use to add tasks to the flow ||
|| **plannedCompletionTime** 
[`integer`](../../data-types.md) | Planned time for task completion in seconds ||
|| **distributionType** 
[`string`](../../data-types.md) | Type of distribution (`manually` for manual distribution, `queue` for queue distribution). 

For more details on distribution types, see [the flow description](./index.md) ||
|| **responsibleQueue** 
[`array`](../../data-types.md) | Array of identifiers of team members in the flow in case of queue distribution. 

Not filled in case of manual distribution ||
|| **manualDistributorId** 
[`integer`](../../data-types.md) | Identifier of the flow moderator (the user who will distribute tasks among employees). 

Not filled in case of queue distribution ||
|| **taskCreators** [`object`](../../data-types.md) | List of users who can add tasks to the flow in the format `{"<object-type>": "<object-identifier>"}`, for example, `[{"user": 3}, {"department": "17:F"}]`. 

The element `{"meta-user": "all-users"}` means that all users can add tasks ||
|| **matchWorkTime** 
[`integer`](../../data-types.md) | Skip weekends and holidays when calculating the task deadline. 

Accepts values `0` and `1`. Default is `1` ||
|| **responsibleCanChangeDeadline** 
[`integer`](../../data-types.md) | Can the responsible person change the task deadline? 

Accepts values `0` and `1`. Default is `0` ||
|| **notifyAtHalfTime** 
[`integer`](../../data-types.md) | Notify the performer at half the task deadline. 

Accepts values `0` and `1`. Default is `0` ||
|| **taskControl** 
[`integer`](../../data-types.md) | Send the completed task to the Creator for review. 

Accepts values `0` and `1`. Default is `0` ||
|| **notifyOnQueueOverflow** 
[`integer`](../../data-types.md) | Notify the flow administrator when the number of tasks in the queue exceeds this parameter. 

Default is `null` (do not notify) ||
|| **notifyOnTasksInProgressOverflow** 
[`integer`](../../data-types.md) | Notify the flow administrator when the number of tasks in progress exceeds this parameter. 

Default is `50` ||
|| **notifyWhenEfficiencyDecreases** 
[`integer`](../../data-types.md) | Notify the flow administrator when efficiency falls below this parameter. 

Default is `null` (do not notify) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
        "flowData": {
            "id": 517,
            "name": "Updated Flow Name",
            "description": "Updated description",
            "plannedCompletionTime": 7200,
            "distributionType": "manually",
            "manualDistributorId": 3,
            "taskCreators": [["meta-user", "all-users"]],
            "matchWorkTime": 1,
            "notifyAtHalfTime": 0,
            "notifyOnQueueOverflow": null,
            "notifyOnTasksInProgressOverflow": 50
        }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.flow.flow.update
    ```

- cURL (oAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
    -d '{
        "flowData": {
            "id": 517,
            "name": "Updated Flow Name",
            "description": "Updated description",
            "plannedCompletionTime": 7200,
            "distributionType": "manually",
            "manualDistributorId": 3,
            "taskCreators": [["meta-user", "all-users"]],
            "matchWorkTime": 1,
            "notifyAtHalfTime": 0,
            "notifyOnQueueOverflow": null,
            "notifyOnTasksInProgressOverflow": 50
        }
    }' \
    https://your-domain.bitrix24.com/rest/tasks.flow.flow.update
    ```

- JS

    ```js
    BX24.callMethod(
        'tasks.flow.flow.update',
        {
            flowData: {
                id: 517,
                name: 'Updated Flow Name',
                description: 'Updated description',
                plannedCompletionTime: 7200,
                distributionType: 'manually',
                manualDistributorId: 3,
                taskCreators: [['meta-user', 'all-users']],
                matchWorkTime: 1,
                notifyAtHalfTime: 0,
                notifyOnQueueOverflow: null,
                notifyOnTasksInProgressOverflow: 50
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

- PHP

    ```php
    require_once('crest.php'); // connect CRest PHP SDK

    $flowData = [
        "id" => 517,
        "name" => "Updated Flow Name",
        "description" => "Updated description",
        "plannedCompletionTime" => 7200,
        "distributionType" => "manually",
        "manualDistributorId" => 3,
        "taskCreators" => [["meta-user", "all-users"]],
        "matchWorkTime" => 1,
        "notifyAtHalfTime" => 0,
        "notifyOnQueueOverflow" => null,
        "notifyOnTasksInProgressOverflow" => 50
    ];

    // execute request to REST API
    $result = CRest::call(
        'tasks.flow.flow.update',
        [
            'flowData' => $flowData
        ]
    );

    // Handle the response from Bitrix24
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
        "name": "Updated Flow Name",
        "description": "Updated description",
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
        "taskAssignees": [{"user":  1}, {"user":  2}, {"user":  3}],
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
[`integer`](../../data-types.md) | Identifier of the created flow ||
|| **creatorId** 
[`integer`](../../data-types.md) | Identifier of the flow creator. Read-only ||
|| **ownerId** 
[`integer`](../../data-types.md) | Identifier of the flow administrator ||
|| **groupId** 
[`integer`](../../data-types.md) | Identifier of the group to which the flow is linked ||
|| **templateId** 
[`integer`](../../data-types.md) | Identifier of the template used to create tasks in the flow ||
|| **efficiency** 
[`integer`](../../data-types.md) | Efficiency of the flow in percentage. Read-only ||
|| **active** 
[`boolean`](../../data-types.md) | Status of the flow's activity ||
|| **plannedCompletionTime** 
[`integer`](../../data-types.md) | Planned time for task completion in seconds ||
|| **activity** 
[`string`](../../data-types.md) | Date and time of the last activity in the flow. Read-only ||
|| **name** 
[`string`](../../data-types.md) | Name of the flow ||
|| **description** 
[`string`](../../data-types.md) | Description of the flow ||
|| **distributionType** 
[`string`](../../data-types.md) | Type of task distribution in the flow ||
|| **responsibleQueue** 
[`array`](../../data-types.md) | Team of the flow, if the distribution type is queue. Empty for manual distribution ||
|| **manualDistributorId** 
[`integer`](../../data-types.md) | Identifier of the flow moderator for manual distribution (`null` for queue distribution) ||
|| **demo** 
[`boolean`](../../data-types.md) | Indicates whether the flow is a demo. System parameter. Read-only ||
|| **responsibleCanChangeDeadline** 
[`boolean`](../../data-types.md) | Can the responsible person change the task deadline? ||
|| **matchWorkTime** 
[`boolean`](../../data-types.md) | Whether to skip weekends and holidays when calculating the task deadline ||
|| **taskControl** 
[`boolean`](../../data-types.md) | Whether to send the completed task to the Creator for review ||
|| **notifyAtHalfTime** 
[`boolean`](../../data-types.md) | Whether to notify the performer at half the task deadline ||
|| **notifyOnQueueOverflow** 
[`integer`](../../data-types.md) | Number of tasks in the queue, exceeding which will send a notification to the flow administrator (if `null`, notifications are disabled) ||
|| **notifyOnTasksInProgressOverflow** 
[`integer`](../../data-types.md) | Number of tasks in progress, exceeding which will send a notification to the flow administrator (if `null`, notifications are disabled) ||
|| **notifyWhenEfficiencyDecreases** 
[`integer`](../../data-types.md) | Efficiency in percentage, below which a notification will be sent to the flow administrator (if `null`, notifications are disabled) ||
|| **taskCreators** 
[`object`](../../data-types.md) | List of users who can add tasks to the flow in the format `{"<object-type>": "<object-identifier>"}`, for example, `[{"user": 3}, {"department": "17:F"}]`. The element `{"meta-user": "all-users"}` means that all users can add tasks ||
|| **taskAssignees** 
[`object`](../../data-types.md) | Participants of the project to which the flow is linked, if the distribution type is manual. Format: `[{"<object-type>": "<object-identifier>"}]` ||
|| **trialFeatureEnabled** 
[`boolean`](../../data-types.md) | Indicates whether the trial period is enabled for the flow. System parameter. Read-only ||
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
|| `0` | Access denied or flow not found | The portal plan may not allow working with flows, or the user may not have permission to modify the flow ||
|| `0` | `Unknown error` | Unknown error ||
|| `0` | `'distributionType': field's value has an invalid value` | Incorrect value for `distributionType` (similarly for other parameters) ||
|| `0` | A flow with this name already exists | ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-flow-flow-create.md)
- [{#T}](./tasks-flow-flow-get.md)
- [{#T}](./tasks-flow-flow-delete.md)
- [{#T}](./tasks-flow-flow-is-exists.md)
- [{#T}](./tasks-flow-flow-activate.md)