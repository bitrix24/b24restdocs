# Create a New Flow tasks.flow.flow.create

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user who is not an extranet user

The method `tasks.flow.flow.create` creates a flow.

The flow must be associated with a group. If the group ID is not provided when creating the flow, a new group will be automatically created, consisting of the creator, administrator, and the flow team.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **flowData***
[`object`](../../data-types.md) | Field values for creating the flow (detailed description provided below) ||
|#

### Parameter flowData

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name*** 
[`string`](../../data-types.md) | The name of the flow. Must be unique for each flow. 

To check the name, you can use the method [tasks.flow.flow.isExists](./tasks-flow-flow-is-exists.md) ||
|| **description** 
[`string`](../../data-types.md) | Description of the flow ||
|| **groupId** 
[`integer`](../../data-types.md) | The ID of the group to which the flow will be linked. 

If not specified, a new group will be automatically created ||
|| **ownerId** 
[`integer`](../../data-types.md) | The ID of the flow administrator. 

If not specified, the creator will be the administrator of the flow ||
|| **templateId** 
[`integer`](../../data-types.md) | The ID of the template that users will use to add tasks to the flow ||
|| **plannedCompletionTime*** 
[`integer`](../../data-types.md) | The planned time to complete the task in seconds ||
|| **distributionType*** 
[`string`](../../data-types.md) | The type of distribution (`manually` for manual distribution, `queue` for queue distribution). 

For more details on distribution types, see [the flow description](./index.md) ||
|| **responsibleQueue** 
[`array`](../../data-types.md) | An array of IDs of team members in the case of queue distribution. 

Not filled in the case of manual distribution ||
|| **manualDistributorId** 
[`integer`](../../data-types.md) | The ID of the flow moderator (the user who will distribute tasks among employees). 

Not filled in the case of queue distribution ||
|| **taskCreators** 
[`object`](../../data-types.md) | A list of users who can add tasks to the flow in the format `{"<object-type>": "<object-id>"}`, for example, `[{"user": 3}, {"department": "17:F"}]`. 

The element `{"meta-user": "all-users"}` means that all users can add tasks ||
|| **matchWorkTime** 
[`integer`](../../data-types.md) | Skip weekends and holidays when calculating the task deadline. 

Accepts values `0` and `1`. Default is `1` ||
|| **responsibleCanChangeDeadline** 
[`integer`](../../data-types.md) | Can the responsible person change the task deadline? 

Accepts values `0` and `1`. Default is `0` ||
|| **notifyAtHalfTime** [`integer`](../../data-types.md) | Notify the performer at half the task deadline. 

Accepts values `0` and `1`. Default is `0` ||
|| **taskControl** 
[`integer`](../../data-types.md) | Send the completed task to the creator for review. 

Accepts values `0` and `1`. Default is `0` ||
|| **notifyOnQueueOverflow** 
[`integer`](../../data-types.md) | Notify the flow administrator when the number of tasks in the queue exceeds this parameter. 

Default is `null` (do not notify) ||
|| **notifyOnTasksInProgressOverflow** 
[`integer`](../../data-types.md) | Notify the flow administrator when the number of tasks in progress exceeds this parameter. 

Default is `null` (do not notify) ||
|| **notifyWhenEfficiencyDecreases** 
[`integer`](../../data-types.md) | Notify the flow administrator when efficiency falls below this parameter. 

Default is `null` (do not notify) ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
        "flowData": {
            "name": "Unique Flow Name",
            "description": "Flow description",
            "plannedCompletionTime": 3600,
            "distributionType": "queue",
            "responsibleQueue": [1, 2],
            "taskCreators": [["meta-user", "all-users"]],
            "matchWorkTime": 0,
            "notifyAtHalfTime": 1
        }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.flow.flow.create
    ```

- cURL (oAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
    -d '{
        "flowData": {
            "name": "Unique Flow Name",
            "description": "Flow description",
            "plannedCompletionTime": 3600,
            "distributionType": "queue",
            "responsibleQueue": [1, 2],
            "taskCreators": [["meta-user", "all-users"]],
            "matchWorkTime": 0,
            "notifyAtHalfTime": 1
        }
    }' \
    https://your-domain.bitrix24.com/rest/tasks.flow.flow.create
    ```

- JS

    ```js
    BX24.callMethod(
        'tasks.flow.flow.create',
        {
            flowData: {
                name: 'Unique Flow Name2323',
                description: 'Flow description',
                plannedCompletionTime: 3600,
                distributionType: 'queue',
                responsibleQueue: [1, 2],
                taskCreators: [['meta-user', 'all-users']],
                matchWorkTime: 0,
                notifyAtHalfTime: 1
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
        "name" => "Unique Flow Name",
        "description" => "Flow description",
        "plannedCompletionTime" => 3600,
        "distributionType" => "queue",
        "responsibleQueue" => [1, 2],
        "taskCreators" => [["meta-user", "all-users"]],
        "matchWorkTime" => 0,
        "notifyAtHalfTime" => 1
    ];

    // execute request to REST API
    $result = CRest::call(
        'tasks.flow.flow.create',
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

HTTP Status: **200**

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
        "plannedCompletionTime": 3600,
        "activity": "2024-08-22T12:17:48+00:00",
        "name": "Unique Flow Name2323",
        "description": "Flow description",
        "distributionType": "queue",
        "responsibleQueue": [
        1,
        2
        ],
        "demo": false,
        "manualDistributorId": null,
        "responsibleCanChangeDeadline": true,
        "matchWorkTime": false,
        "taskControl": false,
        "notifyAtHalfTime": true,
        "notifyOnQueueOverflow": 10,
        "notifyOnTasksInProgressOverflow": null,
        "notifyWhenEfficiencyDecreases": null,
        "taskCreators": [{"meta-user": "all-users"}],
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
[`object`](../../data-types.md) | Object containing flow data ||
|| **id** 
[`integer`](../../data-types.md) | ID of the created flow ||
|| **creatorId** 
[`integer`](../../data-types.md) | ID of the flow creator. Read-only ||
|| **ownerId** 
[`integer`](../../data-types.md) | ID of the flow administrator ||
|| **groupId** 
[`integer`](../../data-types.md) | ID of the group to which the flow is linked ||
|| **templateId** 
[`integer`](../../data-types.md) | ID of the template used to create tasks in the flow ||
|| **efficiency** 
[`integer`](../../data-types.md) | Efficiency of the flow in percentage. Read-only ||
|| **active** 
[`boolean`](../../data-types.md) | Status of the flow's activity ||
|| **plannedCompletionTime** 
[`integer`](../../data-types.md) | Planned time to complete the task in seconds ||
|| **activity** 
[`string`](../../data-types.md) | Date and time of the last activity in the flow. Read-only ||
|| **name** 
[`string`](../../data-types.md) | Name of the flow ||
|| **description** 
[`string`](../../data-types.md) | Description of the flow ||
|| **distributionType** 
[`string`](../../data-types.md) | Type of task distribution in the flow ||
|| **responsibleQueue** 
[`array`](../../data-types.md) | Flow team, if the distribution type is queue. Empty for manual distribution ||
|| **manualDistributorId** 
[`integer`](../../data-types.md) | ID of the flow moderator for manual distribution (`null` for queue distribution) ||
|| **demo** 
[`boolean`](../../data-types.md) | Indicates if the flow is a demo. System parameter. Read-only ||
|| **responsibleCanChangeDeadline** 
[`boolean`](../../data-types.md) | Can the responsible person change the task deadline? ||
|| **matchWorkTime** 
[`boolean`](../../data-types.md) | Whether to skip weekends and holidays when calculating the task deadline ||
|| **taskControl** 
[`boolean`](../../data-types.md) | Whether to send the completed task to the creator for review ||
|| **notifyAtHalfTime** 
[`boolean`](../../data-types.md) | Whether to notify the performer at half the task deadline ||
|| **notifyOnQueueOverflow** 
[`integer`](../../data-types.md) | Number of tasks in the queue, exceeding which will send a notification to the flow administrator (if `null`, notifications are turned off) ||
|| **notifyOnTasksInProgressOverflow** 
[`integer`](../../data-types.md) | Number of tasks in progress, exceeding which will send a notification to the flow administrator (if `null`, notifications are turned off) ||
|| **notifyWhenEfficiencyDecreases** 
[`integer`](../../data-types.md) | Efficiency in percentage, below which a notification will be sent to the flow administrator (if `null`, notifications are turned off) ||
|| **taskCreators** 
[`object`](../../data-types.md) | List of users who can add tasks to the flow in the format `{"<object-type>": "<object-id>"}`, for example `[{"user": 3}, {"department": "17:F"}]`. The element `{"meta-user": "all-users"}` means that all users can add tasks ||
|| **taskAssignees** 
[`object`](../../data-types.md) | Participants of the project to which the flow is linked, if the distribution type is manual ||
|| **trialFeatureEnabled** 
[`boolean`](../../data-types.md) | Indicates if the trial period is enabled for the flow. System parameter. Read-only ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Access denied or flow not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Additional Information**||
|| `0` | Access denied or flow not found | The portal's plan may not allow working with flows or the user may not have permission to create a flow ||
|| `0` | `Unknown error` | Unknown error ||
|| `0` | `'distributionType': field's value has an invalid value` | Invalid value for `distributionType` (similarly for other parameters) ||
|| `0` | A flow with this name already exists | ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-flow-flow-get.md)
- [{#T}](./tasks-flow-flow-update.md)
- [{#T}](./tasks-flow-flow-delete.md)
- [{#T}](./tasks-flow-flow-is-exists.md)
- [{#T}](./tasks-flow-flow-activate.md)