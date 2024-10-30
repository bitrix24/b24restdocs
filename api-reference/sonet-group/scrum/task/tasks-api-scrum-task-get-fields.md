# Get Scrum Task Fields tasks.api.scrum.task.getFields

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves the available fields for a Scrum task.

No parameters.

## Code Examples

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.api.scrum.task.getFields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.api.scrum.task.getFields
    ```

- JS

    ```js
    BX24.callMethod(
        'tasks.api.scrum.task.getFields',
        {},
        function(res)
        {
            console.log(res);
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.api.scrum.task.getFields',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "fields": 
    {
        "entityId": 
        {
            "type": "integer"
        },
        "storyPoints": 
        {
            "type": "string"
        },
        "epicId": 
        {
            "type": "integer"
        },
        "sort": 
        {
            "type": "integer"
        },
        "createdBy": 
        {
            "type": "integer"
        },
        "modifiedBy": 
        {
            "type": "integer"
        }
    }
}
```

### Returned Data {#fields}

The response returns an object `fields`, which contains all the fields of the Scrum task and their types `type`.

#|
|| **Name**
`type` | **Description** ||
|| **entityId**
`integer` | Identifier of the backlog or sprint ||
|| **storyPoints**
`string` | Story Points (relative assessment of task complexity).

Can have a string value ||
|| **epicId**
`integer` | Identifier of the epic ||
|| **sort**
`integer` | Sorting ||
|| **createdBy**
`integer` | Created by whom ||
|| **modifiedBy**
`integer` | Modified by whom ||
|#

## Error Handling

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-task-update.md)
- [{#T}](./tasks-api-scrum-task-get.md)