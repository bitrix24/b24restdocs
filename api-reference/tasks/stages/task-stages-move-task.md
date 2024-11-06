# Move a task from one stage to another task.stages.movetask

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method:
> - any user for stages in "My Planner"
> - any user with access to the group for kanban stages

The method moves a task from one stage to another and allows changing the position of the task within the group's kanban or "My Planner".

The method works as follows:
- If a group stage is provided, the move occurs within the group's kanban
- If "My Planner" stage is provided, the move occurs within it

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Task identifier ||
|| **stageId***
[`integer`](../../data-types.md) | `ID` of the stage to which the task should be moved ||
|| **before**
[`integer`](../../data-types.md) | `ID` of the task before which the task should be placed in the stage ||
|| **after**
[`integer`](../../data-types.md) | `ID` of the task after which the task should be placed in the stage ||
|#

{% note info %}

The `before` and `after` parameters are mutually exclusive. You must specify either one or the other.

If both parameters are not filled, the task is added to the stage column according to the project or "My Planner" settings.

{% endnote %}

## Code Examples

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    "id": 1,
    "stageId": 2,
    "before": 3,
    "after": 4
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/task.stages.movetask
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: YOUR_ACCESS_TOKEN" \
    -d '{
    "id": 1,
    "stageId": 2,
    "before": 3,
    "after": 4
    }' \
    https://your-domain.bitrix24.com/rest/task.stages.movetask
    ```

- JS

    ```js
    const taskId = 1;
    const stageId = 2;
    BX24.callMethod(
        'task.stages.movetask',
        {
            id: taskId,
            stageId: stageId,
            before: 3,
            after: 4
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

    $taskId = 1;
    $stageId = 2;

    // executing request to REST API
    $result = CRest::call(
        'task.stages.movetask',
        [
            'id' => $taskId,
            'stageId' => $stageId,
            'before' => 3,
            'after' => 4
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
[`boolean`](../../data-types.md) | Returns `true` if the stage move was successful
||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED_MOVE",
    "error_description": "You cannot move this task"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED_MOVE` | You cannot move this task ||
|| `TASK_NOT_FOUND` | Task not found or access denied ||
|| `NOT_FOUND` | Stage not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-stages-add.md)
- [{#T}](./task-stages-update.md)
- [{#T}](./task-stages-get.md)
- [{#T}](./task-stages-can-move-task.md)
- [{#T}](./task-stages-delete.md)