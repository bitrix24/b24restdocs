# Get a List of Available Epic Fields tasks.api.scrum.epic.getFields

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.api.scrum.epic.getFields` returns the available fields of an epic.

Without parameters.

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.epic.getFields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    auth=YOUR_ACCESS_TOKEN
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.epic.getFields
    ```

- JS

    ```js
    BX24.callMethod(
        'tasks.api.scrum.epic.getFields',
        {},
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
    'tasks.api.scrum.epic.getFields',
    []
    );

    // Processing the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    }
    else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **400**

```json
{
    "fields":
    {
        "name": {
            "type": "string"
        },
        "description": {
            "type": "string"
        },
        "groupId": {
            "type": "integer"
        },
        "color": {
            "type": "string"
        },
        "files": {
            "type": "array"
        },
        "createdBy": {
            "type": "integer"
        },
        "modifiedBy": {
            "type": "integer"
        }
    }
}
```

## Returned Data

#|
|| **Field** `type` | **Description** ||
|| **name** `string` | Epic name ||
|| **description** `string` | Epic description ||
|| **groupId** `integer` | Identifier of the group (scrum) to which the epic belongs ||
|| **color** `string` | Epic color ||
|| **files** `array` | Array of files attached to the epic ||
|| **createdBy** `integer` | Created by ||
|| **modifiedBy** `integer` | Modified by ||
|#

## Error Handling

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-epic-add.md)
- [{#T}](./tasks-api-scrum-epic-update.md)
- [{#T}](./tasks-api-scrum-epic-get.md)
- [{#T}](./tasks-api-scrum-epic-list.md)
- [{#T}](./tasks-api-scrum-epic-delete.md)