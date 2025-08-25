# Create a new Flow tasks.flow.Flow.create

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user who is not an extranet user

The method `tasks.flow.Flow.create` creates a flow.

The flow must be linked to a group. If a group ID is not provided when creating the flow, a new group will be automatically created, consisting of the creator, administrator, and the flow team.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **flowData***
[`object`](../../data-types.md) | Field values for creating the flow (detailed description is provided below) ||
|#

### Parameter flowData

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | The name of the flow. Must be unique for each flow.

You can check the name using the method [tasks.flow.Flow.isExists](./tasks-flow-flow-is-exists.md) ||
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
[`string`](../../data-types.md) | Distribution type:
- `manually` — manual distribution
- `queue` — queue distribution
- `himself` — self-distribution

More about distribution types can be found in the article [{#T}](./index.md) ||
|| **responsibleList***
[`object`](../../data-types.md) | IDs of employees who will receive tasks.

For manual distribution, specify the ID of the flow moderator.

For self-distribution or queue distribution, specify the IDs of employees or departments. For example:

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

If you do not add the suffix `:F`, the system will select all sub-departments of the specified department according to the company structure ||
|| **taskCreators**
[`object`](../../data-types.md) | A list of users who can add tasks to the flow in the format `{"<entity-type>": "<entity-id>"}`. For example:

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

If you do not add the suffix `:F`, the system will select all sub-departments of the specified department according to the company structure.

To allow all users to add tasks, specify the value `{"meta-user": "all-users"}` ||
|| **matchWorkTime**
[`integer`](../../data-types.md) | Skip weekends and holidays when calculating the task deadline.

Accepts values `0` and `1`. Default is `1` ||
|| **responsibleCanChangeDeadline**
[`integer`](../../data-types.md) | Can the responsible person change the task deadline.

Accepts values `0` and `1`. Default is `0` ||
|| **notifyAtHalfTime**
[`integer`](../../data-types.md) | Notify the assignee at half the task deadline.

Accepts values `0` and `1`. Default is `0` ||
|| **taskControl**
[`integer`](../../data-types.md) | Send the completed task to the creator for review.

Accepts values `0` and `1`. Default is `0` ||
|| **notifyOnQueueOverflow**
[`integer`](../../data-types.md) | Notify the flow administrator when the number of tasks in the queue exceeds this parameter.

Default is `null`, meaning no notifications ||
|| **notifyOnTasksInProgressOverflow**
[`integer`](../../data-types.md) | Notify the flow administrator when the number of tasks in progress exceeds this parameter.

Default is `null`, meaning no notifications ||
|| **notifyWhenEfficiencyDecreases**
[`integer`](../../data-types.md) | Notify the flow administrator when efficiency drops below this parameter.

Default is `null`, meaning no notifications ||
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
            "name": "Unique Flow Name",
            "description": "Flow description",
            "plannedCompletionTime": 7200,
            "distributionType": "manually",
            "responsibleList": [["user","3"]],
            "taskCreators": [["meta-user","all-users"]],
            "matchWorkTime": 1,
            "notifyAtHalfTime": 0
        }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.flow.Flow.create
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
            "plannedCompletionTime": 7200,
            "distributionType": "manually",
            "responsibleList": [["user","3"]],
            "taskCreators": [["meta-user","all-users"]],
            "matchWorkTime": 1,
            "notifyAtHalfTime": 0
        }
    }' \
    https://your-domain.bitrix24.com/rest/tasks.flow.Flow.create
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'tasks.flow.Flow.create',
    		{
    			flowData: {
    				name: 'Unique Flow Name',
    				description: 'Flow description',
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
                'tasks.flow.Flow.create',
                [
                    'flowData' => [
                        'name'                  => 'Unique Flow Name',
                        'description'           => 'Flow description',
                        'plannedCompletionTime' => 7200,
                        'distributionType'      => 'manually',
                        'responsibleList'       => [
                            ['user', '3']
                        ],
                        'taskCreators'          => [
                            ['meta-user', 'all-users']
                        ],
                        'matchWorkTime'         => 1,
                        'notifyAtHalfTime'      => 0
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating flow: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.flow.Flow.create',
        {
            flowData: {
                name: 'Unique Flow Name',
                description: 'Flow description',
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
    require_once('crest.php'); // connect CRest PHP SDK

    $flowData = [
        "name" => "Unique Flow Name",
        "description" => "Flow description",
        "plannedCompletionTime" => 7200,
        "distributionType" => "manually",
        "responsibleList" => [["user", "3"]],
        "taskCreators" => [["meta-user", "all-users"]],
        "matchWorkTime" => 1,
        "notifyAtHalfTime" => 0
    ];

    // execute the request to the REST API
    $result = CRest::call(
        'tasks.flow.Flow.create',
        [
            'flowData' => $flowData
        ]
    );

    // Process the response from Bitrix24
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
|| **responsibleList**
[`array`](../../data-types.md) | List of responsible persons for tasks in the flow. For manual distribution, this is the flow moderator ||
|| **demo**
[`boolean`](../../data-types.md) | Indicates whether the flow is a demo. System parameter. Read-only ||
|| **responsibleCanChangeDeadline**
[`boolean`](../../data-types.md) | Can the responsible person change the task deadline ||
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
[`object`](../../data-types.md) | List of users who can add tasks to the flow in the format `{"<object-type>": "<object-id>"}`. For example, `[{"user": 3}, {"department": "17:F"}]`.

The element `{"meta-user": "all-users"}` means that all users can add tasks ||
|| **team**
[`object`](../../data-types.md) | Flow team.

For manual distribution, this includes all project participants to which the flow is linked, except for the moderator.

For queue and self-distribution, the team is the same as in `responsibleList` ||
|| **trialFeatureEnabled**
[`boolean`](../../data-types.md) | Indicates whether the trial period is enabled for the flow. System parameter. Read-only ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Access denied or flow not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Additional Information** ||
|| `0` | Access denied or flow not found | The account plan does not allow working with flows or the user does not have permission to create a flow ||
|| `0` | `Unknown error` | Unknown error ||
|| `0` | `'distributionType': field's value has an invalid value` | Invalid value for `distributionType` (similarly for other parameters) ||
|| `0` | A flow with this name already exists | ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-flow-flow-update.md)
- [{#T}](./tasks-flow-flow-get.md)
- [{#T}](./tasks-flow-flow-delete.md)
- [{#T}](./tasks-flow-flow-is-exists.md)
- [{#T}](./tasks-flow-flow-activate.md)
- [{#T}](./tasks-flow-flow-pin.md)