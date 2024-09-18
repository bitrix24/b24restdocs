# Delete Sprint tasks.api.scrum.sprint.delete

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to Scrum

The method `tasks.api.scrum.sprint.delete` removes a sprint.

When a sprint with tasks is deleted, the tasks will be moved to the backlog.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the sprint ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 1
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.sprint.delete
    ```

- cURL (oAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "id": 1
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.sprint.delete
    ```

- JS

    ```js
    const sprintId = 1;
    BX24.callMethod(
        'tasks.api.scrum.sprint.delete',
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
        'tasks.api.scrum.sprint.delete',
        [
            'id' => 1,
        ]
    );

    // Processing the response from Bitrix24
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
    "result" : []
}
```

Upon successful deletion, the method returns an empty array.

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
|| `0` | `Sprint not found` | The sprint does not exist ||
|| `0` | `It is forbidden to remove a sprint with items` | Cannot delete a sprint that has tasks ||
|| `0` | `Sprint items have not been moved to backlog` | Failed to move tasks from the sprint to the backlog ||
|| `100` | `Could not find value for parameter {id}` | Incorrect parameter name or parameter not set ||
|| `100` | `Invalid value {stringValue} to match with parameter {id}. Should be value of type int` | Invalid parameter type ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-api-scrum-sprint-add.md)
- [{#T}](./tasks-api-scrum-sprint-update.md)
- [{#T}](./tasks-api-scrum-sprint-start.md)
- [{#T}](./tasks-api-scrum-sprint-complete.md)
- [{#T}](./tasks-api-scrum-sprint-get.md)
- [{#T}](./tasks-api-scrum-sprint-list.md)
- [{#T}](./tasks-api-scrum-sprint-get-fields.md)