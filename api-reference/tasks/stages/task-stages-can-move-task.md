# Check the ability to move a task task.stages.canmovetask

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user for stages of My Plan
> - any user with access to the group for Kanban stages

The method checks if the current user can move tasks in the specified entity.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityId***
[`integer`](../../data-types.md) | `ID` of the entity ||
|| **entityType***
[`string`](../../data-types.md) | Type of entity: 
- `U` — user
- `G` — group

In the case of `U` (My Plan) — the value `true` will only be returned if the `entityId` contains the identifier of the current user ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "entityId": 1,
    "entityType": "U"
    }' \
    https://your-domain.com/rest/_USER_ID_/_CODE_/task.stages.canmovetask
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "entityId": 1,
    "entityType": "U"
    }' \
    https://your-domain.com/rest/task.stages.canmovetask
    ```

- JS

    ```js
    const entityId = 1;
    const entityType = 'U';
    BX24.callMethod(
        'task.stages.canmovetask',
        {
            entityId: entityId,
            entityType: entityType
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

    $entityId = 1;
    $entityType = 'U';

    // executing the request to the REST API
    $result = CRest::call(
        'task.stages.canmovetask',
        [
            'entityId' => $entityId,
            'entityType' => $entityType
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

HTTP Status: **200**

```json
{
    "result": true
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result** 
[`boolean`](../../data-types.md) | Returns `true` if the current user can move the task.

Otherwise — `false`
||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-stages-add.md)
- [{#T}](./task-stages-update.md)
- [{#T}](./task-stages-get.md)
- [{#T}](./task-stages-move-task.md)
- [{#T}](./task-stages-delete.md)