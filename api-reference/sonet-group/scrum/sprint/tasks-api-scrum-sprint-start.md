# Start Sprint tasks.api.scrum.sprint.start

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.api.scrum.sprint.start` initiates a sprint.

Only a planned sprint can be started.

When starting a sprint, the columns of the active sprint and Automation rules will be carried over from the previous completed sprint, if one exists.

Tasks will be added to the kanban of the active sprint. If there were completed tasks in the sprint at that moment, they will be moved to the backlog.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Sprint identifier ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 2
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.sprint.start
    ```

- cURL (oAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "id": 2
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.sprint.start
    ```

- JS

    ```js
    const sprintId = 2;
    BX24.callMethod(
        'tasks.api.scrum.sprint.start',
        {
            id: sprintId
        },
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    // executing a request to the REST API
    $result = CRest::call(
        'tasks.api.scrum.sprint.start',
        [
            'id' => 2,
        ]
    );

    // Handling the response from Bitrix24
    if (isset($result['error'])) {
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
    "result":
    {
        "id": 2,
        "groupId": 143,
        "entityType": "sprint",
        "name": "Sprint 1",
        "goal": "Sprint goal",
        "sort": 1,
        "createdBy": 1,
        "modifiedBy": 1,
        "dateStart": "2024-07-19T15:03:01+00:00",
        "dateEnd": "2024-08-02T15:03:01+00:00",
        "status": "active"
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
[`integer`](../../../data-types.md) | Group identifier (Scrum) to which the sprint belongs ||
|| **entityType** 
[`string`](../../../data-types.md) | Entity type (in this case `sprint`) ||
|| **name** 
[`string`](../../../data-types.md) | Name of the sprint ||
|| **goal** 
[`string`](../../../data-types.md) | Sprint goal. Set only in the interface when starting the sprint ||
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

HTTP status: **400**

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
|| `0` | `Sprint not found` | Such a sprint does not exist ||
|| `0` | `Sprint must be planned` | The sprint must be in the "planned" status ||
|| `100` | `Could not find value for parameter {id}` | Incorrect parameter name or parameter not set ||
|| `100` | `Invalid value {stringValue} to match with parameter {id}. Should be value of type int` | Invalid parameter type ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-api-scrum-sprint-add.md)
- [{#T}](./tasks-api-scrum-sprint-update.md)
- [{#T}](./tasks-api-scrum-sprint-complete.md)
- [{#T}](./tasks-api-scrum-sprint-get.md)
- [{#T}](./tasks-api-scrum-sprint-list.md)
- [{#T}](./tasks-api-scrum-sprint-delete.md)
- [{#T}](./tasks-api-scrum-sprint-get-fields.md)