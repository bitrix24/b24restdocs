# Move Task from One Stage to Another task.stages.movetask

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- no error response is provided
- no success response is provided
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.stages.movetask` moves a task from one stage to another and allows changing the position of the task within the group's Kanban or My Plan.

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **id^*^**
[`integer`](../../data-types.md) | Task identifier. ||
|| **stageId^*^**
[`integer`](../../data-types.md) | ID of the stage to which the task should be moved. ||
|| **before**
[`integer`](../../data-types.md) | ID of the task before which the task should be placed in the stage. ||
|| **after**
[`integer`](../../data-types.md) | ID of the task after which the task should be placed in the stage. ||
|#

{% note info %}

If the `before` and `after` parameters are not provided simultaneously, the task is added to the column according to the project/My Plan settings. Otherwise, `before` and `after` are mutually exclusive. You specify either one or the other parameter as needed.

{% endnote %}

The method works as follows. If a group stage is provided, the movement occurs within the group's Kanban. If a My Plan stage is provided, the movement occurs within it. Before moving, permission checks are performed.

## Examples

{% list tabs %}

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

- cURL (oAuth)
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

- PHP
    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    $taskId = 1;
    $stageId = 2;

    // executing the request to the REST API
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

## Error Handling

HTTP Status: **200**

```json
{
"error": "ACCESS_DENIED_MOVE",
"error_description": "You cannot move this task"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED_MOVE` | You cannot move this task ||
|| `TASK_NOT_FOUND` | Task not found or access to it is denied ||
|| `NOT_FOUND` | Stage not found ||
|#