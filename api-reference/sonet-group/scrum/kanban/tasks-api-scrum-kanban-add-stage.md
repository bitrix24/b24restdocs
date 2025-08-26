# Create a Scrum Kanban Stage tasks.api.scrum.kanban.addStage

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method creates a Scrum Kanban stage.

A Scrum Kanban must include stages of type new `NEW` and final `FINISH`.

Use this method only for active sprints, meaning with the field `"status": "active"`.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Fields corresponding to the available list of fields [tasks.api.scrum.kanban.getFields](./tasks-api-scrum-kanban-get-fields.md) (detailed description provided [below](#parametr-fields)) ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **sprintId***
[`integer`](../../../data-types.md) | Sprint identifier. You can obtain the identifier using the method [tasks.api.scrum.sprint.list](../sprint/tasks-api-scrum-sprint-list.md) ||
|| **name***
[`string`](../../../data-types.md) | Name of the Kanban stage ||
|| **type**
[`string`](../../../data-types.md) | Type of the Kanban stage. Possible values: `NEW`, `WORK`, `FINISH` ||
|| **sort**
[`integer`](../../../data-types.md) | Sort order. The field value must be a multiple of `100` ||
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
    -d '{"fields":{"sprintId":1,"name":"First Stage","type":"NEW","color":"00C4FB","sort":100}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.api.scrum.kanban.addStage
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"sprintId":1,"name":"First Stage","type":"NEW","color":"00C4FB","sort":100},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.api.scrum.kanban.addStage
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'tasks.api.scrum.kanban.addStage',
    		{
    			"fields": {
    				"sprintId": 1,
    				"name": "First Stage",
    				"type": "NEW",
    				"color": "00C4FB",
    				"sort": 100,
    			},
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.api.scrum.kanban.addStage',
                [
                    'fields' => [
                        'sprintId' => 1,
                        'name' => 'First Stage',
                        'type' => 'NEW',
                        'color' => '00C4FB',
                        'sort' => 100,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding stage: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.api.scrum.kanban.addStage',
        {
            "fields": {
                "sprintId": 1,
                "name": "First Stage",
                "type": "NEW",
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.api.scrum.kanban.addStage',
        [
            'fields' => [
                'sprintId' => 1,
                'name' => 'First Stage',
                'type' => 'NEW',
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
    "result": 5,
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
    "error_description": "Sprint not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | `Sprint id not found` | Required field `sprintId` is not filled ||
|| `0` | `Sprint not found` | An unknown sprint identifier was provided ||
|| `0` | `Access denied` | Access is denied ||
|| `0` | `Incorrect name format` | Required field `name` is not filled ||
|| `0` | Unknown error | ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-kanban-update-stage.md)
- [{#T}](./tasks-api-scrum-kanban-add-task.md)
- [{#T}](./tasks-api-scrum-kanban-delete-stage.md)
- [{#T}](./tasks-api-scrum-kanban-delete-task.md)
- [{#T}](./tasks-api-scrum-kanban-get-fields.md)
- [{#T}](./tasks-api-scrum-kanban-get-stages.md)