# Add Task to Scrum Kanban tasks.api.scrum.kanban.addTask

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method adds a task to the Scrum kanban.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **sprintId***
[`integer`](../../../data-types.md) | Sprint identifier. You can obtain the identifier using the [tasks.api.scrum.sprint.list](../sprint/tasks-api-scrum-sprint-list.md) method ||
|| **taskId***
[`integer`](../../../data-types.md) | Task identifier. You can obtain the identifier using the [tasks.task.list](../../../tasks/tasks-task-list.md) method ||
|| **stageId***
[`integer`](../../../data-types.md) | Stage identifier. You can obtain the identifier using the [tasks.api.scrum.kanban.getStages](./tasks-api-scrum-kanban-get-stages.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"sprintId":5,"taskId":751,"stageId":58}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.api.scrum.kanban.addTask
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"sprintId":5,"taskId":751,"stageId":58,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.api.scrum.kanban.addTask
    ```

- JS

    ```js
    BX24.callMethod(
        'tasks.api.scrum.kanban.addTask',
        {
            "sprintId": 5,
            "taskId": 751,
            "stageId": 58,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.api.scrum.kanban.addTask',
        [
            'sprintId' => 5,
            'taskId' => 751,
            'stageId' => 58
        ]
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
    "result": true,
    "time": {
        "start": 1712137817.343984,
        "finish": 1712137817.605804,
        "duration": 0.26182007789611816,
        "processing": 0.018325090408325195,
        "date_start": "2024-04-03T12:50:17+02:00",
        "date_finish": "2024-04-03T12:50:17+02:00"
    }
}
```

## Error Handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description": "TaskId id not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | `Sprint id not found` | Required field `sprintId` is not filled ||
|| `0` | `TaskId id not found` | Required field `taskId` is not filled ||
|| `0` | `StageId id not found` | Required field `stageId` is not filled ||
|| `0` | `Sprint not found` | An unknown sprint identifier was provided ||
|| `0` | `Stage not found` | An unknown stage identifier was provided ||
|| `0` | `Task not found. The task must be with GROUP_ID` | An unknown task identifier was provided or the task does not belong to the sprint group ||
|| `0` | `Access denied` | Access is denied ||
|| `0` | Unknown error | ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-kanban-add-stage.md)
- [{#T}](./tasks-api-scrum-kanban-update-stage.md)
- [{#T}](./tasks-api-scrum-kanban-delete-stage.md)
- [{#T}](./tasks-api-scrum-kanban-delete-task.md)
- [{#T}](./tasks-api-scrum-kanban-get-fields.md)
- [{#T}](./tasks-api-scrum-kanban-get-stages.md)