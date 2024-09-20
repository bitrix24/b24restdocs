# Get Sprint Fields by Its Identifier tasks.api.scrum.sprint.get

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.api.scrum.sprint.get` returns the values of the sprint fields by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **sprintId***
[`integer`](../../../data-types.md) | Identifier of the sprint.

The identifier can be obtained using the method [tasks.api.scrum.sprint.list](./tasks-api-scrum-sprint-list.md) ||
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
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.sprint.get
    ```

- cURL (oAuth)
  
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 2,
    "auth": "YOUR_ACCESS_TOKEN"
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.sprint.get
    ```

- JS
  
    ```js
    const sprintId = 2;
    BX24.callMethod(
        'tasks.api.scrum.sprint.get',
        {
            id: sprintId,
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

    $sprintId = 2;

    // executing the request to the REST API
    $result = CRest::call(
        'tasks.api.scrum.sprint.get',
        [
            'id' => $sprintId
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
    "result":
    {
        "id": 2,
        "groupId": 143,
        "entityType": "sprint",
        "name": "Sprint 1",
        "goal": "",
        "sort": 1,
        "createdBy": 1,
        "modifiedBy": 1,
        "dateStart": "2024-07-19T15:03:01+00:00",
        "dateEnd": "2024-08-02T15:03:01+00:00",
        "status": "planned"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result** 
[`object`](../../../data-types.md) | Object containing data about the sprint ||
|| **id** 
[`integer`](../../../data-types.md) | Identifier of the sprint ||
|| **groupId** 
[`integer`](../../../data-types.md) | Identifier of the group (Scrum) to which the sprint belongs ||
|| **entityType** 
[`string`](../../../data-types.md) | Type of entity (in this case `sprint`) ||
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
|| `0` | `Access denied` | No access to view sprint data ||
|| `0` | `Sprint not found` | The sprint does not exist ||
|| `100` | `Could not find value for parameter {id}` | Incorrect parameter name or parameter not set ||
|| `100` | `Invalid value {stringValue} to match with parameter {id}. Should be value of type int` | Invalid parameter type ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-api-scrum-sprint-add.md)
- [{#T}](./tasks-api-scrum-sprint-update.md)
- [{#T}](./tasks-api-scrum-sprint-start.md)
- [{#T}](./tasks-api-scrum-sprint-complete.md)
- [{#T}](./tasks-api-scrum-sprint-list.md)
- [{#T}](./tasks-api-scrum-sprint-delete.md)
- [{#T}](./tasks-api-scrum-sprint-get-fields.md)