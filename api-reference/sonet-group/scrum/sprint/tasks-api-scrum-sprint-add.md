# Add Sprint in Scrum tasks.api.scrum.sprint.add

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to Scrum

The method `tasks.api.scrum.sprint.add` adds a sprint to Scrum.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object containing sprint data ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **groupId***
[`integer`](../../../data-types.md) | Identifier of the group (Scrum) to which the sprint belongs. 

The identifier can be obtained using the method [tasks.api.scrum.sprint.get](./tasks-api-scrum-sprint-get.md) for an existing sprint ||
|| **name***
[`string`](../../../data-types.md) | Name of the sprint ||
|| **sort**
[`integer`](../../../data-types.md) | Sorting ||
|| **dateStart**
[`string`](../../../data-types.md) | Start date of the sprint. Available formats: `ISO 8601`, `timestamp` ||
|| **dateEnd**
[`string`](../../../data-types.md) | End date of the sprint. Available formats: `ISO 8601`, `timestamp` ||
|| **status**
[`string`](../../../data-types.md) | Status of the sprint. Available values: `active`, `planned`, `completed` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "fields": {
        "name": "Sprint 1",
        "groupId": 1,
        "createdBy": 1,
        "sort": 1,
        "status": "planned",
        "dateStart": "2021-11-22T00:00:00+02:00",
        "dateEnd": "2021-11-29T00:00:00+02:00"
    }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.sprint.add
    ```

- cURL (oAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "fields": {
        "name": "Sprint 1",
        "groupId": 1,
        "createdBy": 1,
        "sort": 1,
        "status": "planned",
        "dateStart": "2021-11-22T00:00:00+02:00",
        "dateEnd": "2021-11-29T00:00:00+02:00"
    }
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.sprint.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'tasks.api.scrum.sprint.add',
    		{
    			fields: {
    				name: name,
    				groupId: groupId,
    				createdBy: createdBy,
    				sort: sort,
    				status: status,
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
                'tasks.api.scrum.sprint.add',
                [
                    'fields' => [
                        'name'      => $name,
                        'groupId'   => $groupId,
                        'createdBy' => $createdBy,
                        'sort'      => $sort,
                        'status'    => $status,
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
        echo 'Error adding sprint: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const groupId = 1;
    const name = 'Sprint 1';
    const createdBy = 1;
    const sort = 1;
    const status = 'planned';
    const dateStart = '2021-11-22T00:00:00+02:00';
    const dateEnd = '2021-11-29T00:00:00+02:00';
    BX24.callMethod(
        'tasks.api.scrum.sprint.add',
        {
            fields: {
                name: name,
                groupId: groupId,
                createdBy: createdBy,
                sort: sort,
                status: status,
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

    $groupId = 1;
    $name = 'Sprint 1';
    $createdBy = 1;
    $sort = 1;
    $status = 'planned';
    $dateStart = '2021-11-22T00:00:00+02:00';
    $dateEnd = '2021-11-29T00:00:00+02:00';

    // execute request to REST API
    $result = CRest::call(
        'tasks.api.scrum.sprint.add',
        [
            'fields' => [
                'name' => $name,
                'groupId' => $groupId,
                'createdBy' => $createdBy,
                'sort' => $sort,
                'status' => $status,
                'dateStart' => $dateStart,
                'dateEnd' => $dateEnd
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
        "id": 1,
        "groupId": 1,
        "entityType": "sprint",
        "name": "Sprint 1",
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
[`integer`](../../../data-types.md) | Identifier of the sprint ||
|| **groupId**
[`integer`](../../../data-types.md) | Identifier of the group (Scrum) to which the sprint belongs ||
|| **entityType**
[`string`](../../../data-types.md) | Entity type (in this case `sprint`) ||
|| **name**
[`string`](../../../data-types.md) | Name of the sprint ||
|| **goal**
[`string`](../../../data-types.md) | Goal of the sprint. Set only in the interface when starting the sprint ||
|| **sort**
[`integer`](../../../data-types.md) | Sorting ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user who created the sprint ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user who modified the sprint ||
|| **dateStart**
[`string`](../../../data-types.md) | Start date of the sprint in `ISO 8601` format ||
|| **dateEnd**
[`string`](../../../data-types.md) | End date of the sprint in `ISO 8601` format ||
|| **status**
[`string`](../../../data-types.md) | Status of the sprint ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 0,
    "error_description": "Sprint not created"
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
|| `100` | `Could not find value for parameter {fields}` | Incorrect parameter name or parameter not set ||
|| `100` | `Invalid value {stringValue} to match with parameter {fields}. Should be value of type array` | Invalid parameter type ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-api-scrum-sprint-update.md)
- [{#T}](./tasks-api-scrum-sprint-start.md)
- [{#T}](./tasks-api-scrum-sprint-complete.md)
- [{#T}](./tasks-api-scrum-sprint-get.md)
- [{#T}](./tasks-api-scrum-sprint-list.md)
- [{#T}](./tasks-api-scrum-sprint-delete.md)
- [{#T}](./tasks-api-scrum-sprint-get-fields.md)