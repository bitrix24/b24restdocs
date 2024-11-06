# Delete a Kanban or "My Planner" Stage task.stages.delete

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user for "My Planner" stages
> - any user with access to the group for Kanban stages

This method deletes a Kanban or "My Planner" stage.

It takes the `id` of the stage as input. The stage is checked for sufficient access permission, as well as for the absence of tasks within it.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../data-types.md) | Identifier of the stage to be deleted ||
|| **isAdmin**
[`boolean`](../../data-types.md) | If set to `true`, permission checks will not occur, provided the requester is an account administrator ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 5
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/task.stages.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "id": 5
    }' \
    https://your-domain.bitrix24.com/rest/task.stages.delete
    ```
- JS

    ```js
    const stageId = 5;
    BX24.callMethod(
        'task.stages.delete',
        {
            id: stageId,
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

    $stageId = 5;

    // executing the request to the REST API
    $result = CRest::call(
        'task.stages.delete',
        [
            'id' => $stageId
        ]
    );

    // Handling the response from Bitrix24
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
[`boolean`](../../data-types.md) | Returns `true` if the stage was successfully deleted
||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "CANT_DELETE_FIRST",
    "error_description": "Cannot delete the first stage. Move the stage to delete it"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | You cannot manage stages ||
|| `CANT_DELETE_FIRST` | Cannot delete the first stage. Move the stage to delete it ||
|| `IS_SYSTEM` | The default stage cannot be deleted ||
|| `NOT_FOUND` | Stage not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-stages-add.md)
- [{#T}](./task-stages-update.md)
- [{#T}](./task-stages-get.md)
- [{#T}](./task-stages-can-move-task.md)
- [{#T}](./task-stages-move-task.md)