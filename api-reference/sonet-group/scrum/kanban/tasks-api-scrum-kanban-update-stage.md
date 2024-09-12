# Change Scrum Kanban Stage tasks.api.scrum.kanban.updateStage

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method changes the stage of the Scrum Kanban.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **stageId***
[`integer`](../../../data-types.md) | Identifier of the stage. You can obtain the identifier using the method [tasks.api.scrum.kanban.getStages](./tasks-api-scrum-kanban-get-stages.md) ||
|| **fields***
[`object`](../../../data-types.md) | Fields corresponding to the available list of fields [tasks.api.scrum.kanban.getFields](./tasks-api-scrum-kanban-get-fields.md) (detailed description provided [below](#parametr-fields)) ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **sprintId**
[`integer`](../../../data-types.md) | Identifier of the sprint. You can obtain the identifier using the method [tasks.api.scrum.sprint.list](../sprint/tasks-api-scrum-sprint-list.md) ||
|| **name**
[`string`](../../../data-types.md) | Name of the Kanban stage ||
|| **type**
[`string`](../../../data-types.md) | Type of the Kanban stage. Possible values: `NEW`, `WORK`, `FINISH` ||
|| **sort**
[`integer`](../../../data-types.md) | Sorting order. The value of the field must be a multiple of `100` ||
|| **color**
[`string`](../../../data-types.md) | Color of the Kanban stage ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"stageId":65,"fields":{"name":"Updated Stage","type":"WORK","color":"00C4FB","sort":100}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.api.scrum.kanban.updateStage
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"stageId":65,"fields":{"name":"Updated Stage","type":"WORK","color":"00C4FB","sort":100},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.api.scrum.kanban.updateStage
    ```

- JS

    ```js
    BX24.callMethod(
        'tasks.api.scrum.kanban.updateStage',
        {
            "stageId": 65,
            "fields": {
                "name": "Updated Stage",
                "type": "WORK",
                "color": "00C4FB",
                "sort": 100,
            },
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
        'tasks.api.scrum.kanban.updateStage',
        [
            'stageId' => 65,
            'fields' => [
                'name' => 'Updated Stage',
                'type' => 'WORK',
                'sort' => 100,
                'color' => '00C4FB',
            ],
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
|| `0` | `Stage id not found`

The required field `stageId` is not filled ||
|| `0` | `Stage not found`

An unknown stage identifier `stageId` was provided ||
|| `0` | `Incorrect sprintId value`

An unknown sprint identifier was provided or there is no access to the sprint ||
|| `0` | `Access denied`

Access is forbidden ||
|| `0` | Unknown error ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-kanban-add-stage.md)
- [{#T}](./tasks-api-scrum-kanban-add-task.md)
- [{#T}](./tasks-api-scrum-kanban-delete-stage.md)
- [{#T}](./tasks-api-scrum-kanban-delete-task.md)
- [{#T}](./tasks-api-scrum-kanban-get-fields.md)
- [{#T}](./tasks-api-scrum-kanban-get-stages.md)