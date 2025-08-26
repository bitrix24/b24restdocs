# Get Kanban Stages by Sprint ID tasks.api.scrum.kanban.getStages

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns the Kanban stages by the sprint ID.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **sprintId***
[`integer`](../../../data-types.md) | The identifier of the sprint. You can obtain the identifier using the method [tasks.api.scrum.sprint.list](../sprint/tasks-api-scrum-sprint-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"sprintId":5}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.api.scrum.kanban.getStages
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"sprintId":5,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.api.scrum.kanban.getStages
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'tasks.api.scrum.kanban.getStages',
    		{
    			"sprintId": 5,
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
                'tasks.api.scrum.kanban.getStages',
                [
                    'sprintId' => 5,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting stages: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.api.scrum.kanban.getStages',
        {
            "sprintId": 5,
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
        'tasks.api.scrum.kanban.getStages',
        [
            'sprintId' => 5,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}


## Response Handling

HTTP Status: **200**

```json
{
    "result": [
      {
        "id": "58",
        "name": "New",
        "sort": "100",
        "type": "NEW",
        "sprintId": "5",
        "color": "00C4FB"
      },
      {
        "id": "59",
        "name": "In Progress",
        "sort": "200",
        "type": "WORK",
        "sprintId": "5",
        "color": "47D1E2"
      },
      {
        "id": "60",
        "name": "Done",
        "sort": "300",
        "type": "FINISH",
        "sprintId": "5",
        "color": "75D900"
      }
    ],
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

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | The identifier of the stage ||
|| **name***
[`string`](../../../data-types.md) | The name of the Kanban stage ||
|| **type**
[`string`](../../../data-types.md) | The type of the Kanban stage. Possible values: `NEW`, `WORK`, `FINISH` ||
|| **sort**
[`integer`](../../../data-types.md) | The sorting order ||
|| **color**
[`string`](../../../data-types.md) | The color of the Kanban stage ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 0,
    "error_description": "Sprint id not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | `Sprint id not found` | The required field `sprintId` is not filled ||
|| `0` | `Sprint not found` | An unknown sprint identifier `sprintId` was provided ||
|| `0` | `Access denied` | Access is denied ||
|| `0` | Unknown error | ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-kanban-add-stage.md)
- [{#T}](./tasks-api-scrum-kanban-update-stage.md)
- [{#T}](./tasks-api-scrum-kanban-add-task.md)
- [{#T}](./tasks-api-scrum-kanban-delete-stage.md)
- [{#T}](./tasks-api-scrum-kanban-delete-task.md)
- [{#T}](./tasks-api-scrum-kanban-get-fields.md)