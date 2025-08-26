# Update Sprint tasks.api.scrum.sprint.update

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to Scrum

The method `tasks.api.scrum.sprint.update` updates a sprint.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Sprint identifier ||
|| **fields***
[`object`](../../../data-types.md) | Object containing sprint data ||
|#

### Parameter fields

#|
|| **Name**
`type` | **Description** ||
|| **groupId** 
[`integer`](../../../data-types.md) | Identifier of the group (Scrum) to which the sprint belongs. 

The identifier can be obtained using the method [tasks.api.scrum.sprint.get](./tasks-api-scrum-sprint-get.md) for an existing sprint ||
|| **name** 
[`string`](../../../data-types.md) | Sprint name ||
|| **sort** 
[`integer`](../../../data-types.md) | Sorting ||
|| **dateStart** 
[`string`](../../../data-types.md) | Sprint start date. Available formats: `ISO 8601`, `timestamp` ||
|| **dateEnd** 
[`string`](../../../data-types.md) | Sprint end date. Available formats: `ISO 8601`, `timestamp` ||
|| **status** 
[`string`](../../../data-types.md) | Sprint status. Available values: `active`, `planned`, `completed` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 2,
    "fields": {
        "name": "Sprint 2",
        "groupId": 1,
        "dateStart": "2021-11-22T00:00:00+02:00",
        "dateEnd": "2021-11-29T00:00:00+02:00"
    }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.sprint.update
    ```

- cURL (oAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "id": 2,
    "fields": {
        "name": "Sprint 2",
        "groupId": 1,
        "dateStart": "2021-11-22T00:00:00+02:00",
        "dateEnd": "2021-11-29T00:00:00+02:00"
    }
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.sprint.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'tasks.api.scrum.sprint.update',
    		{
    			id: sprintId,
    			fields: {
    				name: name,
    				groupId: groupId,
    				dateStart: dateStart,
    				dateEnd: dateEnd,
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.api.scrum.sprint.update',
                [
                    'id' => $sprintId,
                    'fields' => [
                        'name'      => $name,
                        'groupId'   => $groupId,
                        'dateStart' => $dateStart,
                        'dateEnd'   => $dateEnd,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating sprint: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const sprintId = 2;
    const groupId = 1;
    const name = 'Sprint 2';
    const dateStart = '2021-11-22T00:00:00+02:00';
    const dateEnd = '2021-11-29T00:00:00+02:00';
    BX24.callMethod(
        'tasks.api.scrum.sprint.update',
        {
            id: sprintId,
            fields: {
                name: name,
                groupId: groupId,
                dateStart: dateStart,
                dateEnd: dateEnd,
            }
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php'); // connect CRest PHP SDK

    // execute request to REST API
    $result = CRest::call(
        'tasks.api.scrum.sprint.update',
        [
            'id' => 2,
            'fields' => [
                'name' => 'Sprint 2',
                'groupId' => 1,
                'dateStart' => '2021-11-22T00:00:00+02:00',
                'dateEnd' => '2021-11-29T00:00:00+02:00'
            ]
        ]
    );

    // Process response from Bitrix24
    if (isset($result['error'])) {
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
    "result":
    {
        "id": 2,
        "groupId": 1,
        "entityType": "sprint",
        "name": "Sprint 2",
        "goal": "",
        "sort": 1,
        "createdBy": 1,
        "modifiedBy": 1,
        "dateStart": "2021-11-22T00:00:00+02:00",
        "dateEnd": "2021-11-29T00:00:00+02:00",
        "status": "planned"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result** 
[`object`](../../../data-types.md) | Object containing sprint data ||
|| **id** 
[`integer`](../../../data-types.md) | Sprint identifier ||
|| **groupId** 
[`integer`](../../../data-types.md) | Identifier of the group (Scrum) to which the sprint belongs ||
|| **entityType** 
[`string`](../../../data-types.md) | Entity type (in this case `sprint`) ||
|| **name** 
[`string`](../../../data-types.md) | Sprint name ||
|| **goal** 
[`string`](../../../data-types.md) | Sprint goal. Set only in the interface when starting the sprint ||
|| **sort** 
[`integer`](../../../data-types.md) | Sorting ||
|| **createdBy** 
[`integer`](../../../data-types.md) | Identifier of the user who created the sprint ||
|| **modifiedBy** 
[`integer`](../../../data-types.md) | Identifier of the user who modified the sprint ||
|| **dateStart** 
[`string`](../../../data-types.md) | Sprint start date in `ISO 8601` format ||
|| **dateEnd** 
[`string`](../../../data-types.md) | Sprint end date in `ISO 8601` format ||
|| **status** 
[`string`](../../../data-types.md) | Sprint status ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 0,
    "error_description": "Sprint not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `0` | `Access denied` | No access to Scrum ||
|| `0` | `Sprint not created` | Failed to create sprint ||
|| `0` | `Incorrect dateStart format` | Invalid start date format for the sprint ||
|| `0` | `Incorrect dateEnd format` | Invalid end date format for the sprint ||
|| `0` | `createdBy user not found` | User in the "creator" field not found ||
|| `0` | `modifiedBy user not found` | User in the "last modified by" field not found ||
|| `0` | `Unable to add two active sprints` | There cannot be two sprints with the status "active" in the group ||
|| `0` | `Incorrect sprint status` | Status is not in the list of available sprint statuses ||
|| `100` | `Could not find value for parameter {fields}` | Incorrect parameter name or parameter not set ||
|| `100` | `Invalid value {stringValue} to match with parameter {fields}. Should be value of type array` | Invalid parameter type ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-api-scrum-sprint-add.md)
- [{#T}](./tasks-api-scrum-sprint-start.md)
- [{#T}](./tasks-api-scrum-sprint-complete.md)
- [{#T}](./tasks-api-scrum-sprint-get.md)
- [{#T}](./tasks-api-scrum-sprint-list.md)
- [{#T}](./tasks-api-scrum-sprint-delete.md)
- [{#T}](./tasks-api-scrum-sprint-get-fields.md)