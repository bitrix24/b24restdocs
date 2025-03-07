# Update Flow tasks.flow.Flow.update

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: the creator or administrator of the flow

The method `tasks.flow.Flow.update` modifies the flow.

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

You can obtain the identifier using the method for creating a new flow [tasks.flow.Flow.create](./tasks-flow-flow-create.md) or by retrieving a task [tasks.task.get](../tasks-task-get.md) for a task from the flow ||
|| **name** 
[`string`](../../data-types.md) | Name of the flow. Must be unique for each flow. 

To check the name, you can use the method [tasks.flow.Flow.isExists](./tasks-flow-flow-is-exists.md) ||
|| **description** 
[`string`](../../data-types.md) | Description of the flow ||
|| **groupId** 
[`integer`](../../data-types.md) | Identifier of the group to which the flow will be attached. 

If not specified, a new group is automatically created ||
|| **ownerId** 
[`integer`](../../data-types.md) | Identifier of the flow administrator. 

If not specified, the creator of the flow will be the administrator ||
|| **templateId** 
[`integer`](../../data-types.md) | Identifier of the template that users will use to add tasks to the flow ||
|| **plannedCompletionTime*** 
[`integer`](../../data-types.md) | Planned time to complete the task in seconds ||

|| **distributionType*** 
[`string`](../../data-types.md) | Type of distribution:
- `manually` — manual distribution
- `queue` — distribution by queue
- `himself` — self-distribution

More about distribution types can be found in the article [{#T}](./index.md) ||
|| **responsibleList*** 
[`array`](../../data-types.md) | Identifiers of employees who will receive tasks.

For manual distribution, specify the identifier of the flow moderator.

For self-distribution or queue distribution, specify the identifiers of employees or departments. For example:

```js
[
    {
        "department": 3
    },
    {
        "department": "17:F"
    }
]
``` 

If you do not add the suffix `:F`, the system will select all sub-departments of the specified department according to the company structure ||
|| **taskCreators** 
[`object`](../../data-types.md) | List of users who can add tasks to the flow in the format `{"<entity-type>": "<entity-identifier>"}`. For example:

```js
[
    {
        "user": 3
    },
    {
        "department": "17:F"
    }
]
```

If you do not add the suffix `:F`, the system will select all sub-departments of the specified department according to the company structure.

To allow all users to add tasks, specify the value `{"meta-user": "all-users"}` ||
|| **matchWorkTime** 
[`integer`](../../data-types.md) | Skip weekends and holidays when calculating the task deadline. 

Accepts values `0` and `1`. Default is `1` ||
|| **responsibleCanChangeDeadline** 
[`integer`](../../data-types.md) | Can the responsible person change the task deadline? 

Accepts values `0` and `1`. Default is `0` ||
|| **notifyAtHalfTime** 
[`integer`](../../data-types.md) | Notify the assignee at half the task deadline. 

Accepts values `0` and `1`. Default is `0` ||
|| **taskControl** 
[`integer`](../../data-types.md) | Send the completed task to the creator for review. 

Accepts values `0` and `1`. Default is `0` ||
|| **notifyOnQueueOverflow** 
[`integer`](../../data-types.md) | Notify the flow administrator when the number of tasks in the queue exceeds this parameter. 

Default is `null` (do not notify) ||
|| **notifyOnTasksInProgressOverflow** 
[`integer`](../../data-types.md) | Notify the flow administrator when the number of tasks in progress exceeds this parameter. 

Default is `50` ||
|| **notifyWhenEfficiencyDecreases** 
[`integer`](../../data-types.md) | Notify the flow administrator when efficiency drops below this parameter. 

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
            "responsibleList": [{"user":"3"}],
            "taskCreators": [{"meta-user":"all-users"}],
            "matchWorkTime": 1,
            "notifyAtHalfTime": 0
        }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.flow.Flow.update
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
            "responsibleList": [{"user":"3"}],
            "taskCreators": [{"meta-user":"all-users"}],
            "matchWorkTime": 1,
            "notifyAtHalfTime": 0
        }
    }' \
    https://your-domain.bitrix24.com/rest/tasks.flow.Flow.update
    ```

- JS

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
                    {
                        'user':'3'
                    }
                ],
                taskCreators: [
                    {
                        'meta-user':'all-users'
                    }
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

- PHP

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

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

    // executing the request to the REST API
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
[`integer`](../../data-types.md) | Identifier of the created flow ||
|| **creatorId** 
[`integer`](../../data-types.md) | Identifier of the flow creator. Read-only ||
|| **ownerId** 
[`integer`](../../data-types.md) | Identifier of the flow administrator ||
|| **groupId** 
[`integer`](../../data-types.md) | Identifier of the group to which the flow is attached ||
|| **templateId** 
[`integer`](../../data-types.md) | Identifier of the template used to create tasks in the flow ||
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
|| **responsibleList** 
[`array`](../../data-types.md) | List of those responsible for tasks in the flow. For manual distribution, this is the flow moderator ||
|| **demo** 
[`boolean`](../../data-types.md) | Indicates whether the flow is a demo. System parameter. Read-only ||
|| **responsibleCanChangeDeadline** 
[`boolean`](../../data-types.md) | Can the responsible person change the task deadline? ||
|| **matchWorkTime** 
[`boolean`](../../data-types.md) | Whether to skip weekends and holidays when calculating the task deadline ||
|| **taskControl** 
[`boolean`](../../data-types.md) | Whether to send the completed task to the creator for review ||
|| **notifyAtHalfTime** 
[`boolean`](../../data-types.md) | Whether to notify the assignee at half the task deadline ||
|| **notifyOnQueueOverflow** 
[`integer`](../../data-types.md) | Number of tasks in the queue, exceeding which will send a notification to the flow administrator (if `null`, notifications are disabled) ||
|| **notifyOnTasksInProgressOverflow** 
[`integer`](../../data-types.md) | Number of tasks in progress, exceeding which will send a notification to the flow administrator (if `null`, notifications are disabled) ||
|| **notifyWhenEfficiencyDecreases** 
[`integer`](../../data-types.md) | Efficiency in percentage, below which a notification will be sent to the flow administrator (if `null`, notifications are disabled) ||
|| **taskCreators** 
[`object`](../../data-types.md) | List of users who can add tasks to the flow in the format `{"<object-type>": "<object-identifier>"}`. For example, `[{"user": 3}, {"department": "17:F"}]`.

The element `{"meta-user": "all-users"}` means that all users can add tasks ||
|| **team** 
[`object`](../../data-types.md) | Team of the flow.

For manual distribution, this includes all project participants to which the flow is attached, except for the moderator. 

For queue and self-distribution, the team is the same as in `responsibleList` ||
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
|| `0` | Access denied or flow not found | The account plan does not allow working with flows or the user does not have permission to modify the flow ||
|| `0` | `Unknown error` | Unknown error ||
|| `0` | `'distributionType': field's value has an invalid value` | Invalid value for `distributionType`. Similar for other parameters ||
|| `0` | A flow with this name already exists | ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-flow-flow-create.md)
- [{#T}](./tasks-flow-flow-get.md)
- [{#T}](./tasks-flow-flow-delete.md)
- [{#T}](./tasks-flow-flow-is-exists.md)
- [{#T}](./tasks-flow-flow-activate.md)
- [{#T}](./tasks-flow-flow-pin.md)