# Delete Task from Scrum Kanban tasks.api.scrum.kanban.deleteTask

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method removes a task from the Scrum kanban.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **sprintId***
[`integer`](../../../data-types.md) | Identifier of the sprint. You can obtain the identifier using the method [tasks.api.scrum.sprint.list](../sprint/tasks-api-scrum-sprint-list.md) ||
|| **taskId***
[`integer`](../../../data-types.md) | Identifier of the task. You can obtain the identifier using the method [tasks.task.list](../../../tasks/tasks-task-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"sprintId":5,"taskId":751}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.api.scrum.kanban.deleteTask
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"sprintId":5,"taskId":751,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.api.scrum.kanban.deleteTask
    ```

- JS

    ```js
    BX24.callMethod(
        'tasks.api.scrum.kanban.deleteTask',
        {
            "sprintId": 5,
            "taskId": 751,
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
        'tasks.api.scrum.kanban.deleteTask',
        [
            'sprintId' => 5,
            'taskId' => 751,
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
    "time":{
        "start":1712137817.343984,
        "finish":1712137817.605804,
        "duration":0.26182007789611816,
        "processing":0.018325090408325195,
        "date_start":"2024-04-03T12:50:17+03:00",
        "date_finish":"2024-04-03T12:50:17+03:00"
    }
}
```

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_CODE",
    "error_description":"ACTION_NOT_ALLOWED"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `0` | `Sprint id not found`

The required field `sprintId` is not filled ||
|| `0` | `TaskId id not found`

The required field `taskId` is not filled ||
|| `0` | `Sprint not found`

An unknown sprint identifier was provided ||
|| `0` | `Task not found. The task must be with GROUP_ID`

An unknown task identifier was provided or the task does not belong to the sprint group ||
|| `0` | `Access denied`

Access is denied ||
|| `0` | Unknown error ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-kanban-add-stage.md)
- [{#T}](./tasks-api-scrum-kanban-update-stage.md)
- [{#T}](./tasks-api-scrum-kanban-add-task.md)
- [{#T}](./tasks-api-scrum-kanban-delete-stage.md)
- [{#T}](./tasks-api-scrum-kanban-get-fields.md)
- [{#T}](./tasks-api-scrum-kanban-get-stages.md)