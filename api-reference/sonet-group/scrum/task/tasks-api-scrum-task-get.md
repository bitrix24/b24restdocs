# Get Scrum Task Fields by ID tasks.api.scrum.task.get

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to Scrum

This method retrieves the values of the Scrum task fields by its identifier `id`.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Task identifier ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.api.scrum.task.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.api.scrum.task.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'tasks.api.scrum.task.get',
    		{
    			id: 1
    		}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.dir(result);
    	}
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.api.scrum.task.get',
                [
                    'id' => 1
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting scrum task: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.api.scrum.task.get',
        {
            id: 1
        },
            function(result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.api.scrum.task.get',
        [
            'id' => 1
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
    "result": {
        "entityId": 2,
        "storyPoints": "2",
        "epicId": 4,
        "sort": 1,
        "createdBy": 1,
        "modifiedBy": 1
    },
    "time": {
        "start": 1721402687.900315,
        "finish": 1721402694.313811,
        "duration": 6.413496017456055,
        "processing": 6.387248992919922,
        "date_start": "2024-07-19T15:24:47+00:00",
        "date_finish": "2024-07-19T15:24:54+00:00",
        "operating": 6.387217998504639
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with task data ||
|| **entityId** 
[`integer`](../../../data-types.md) | Identifier of the backlog or sprint ||
|| **storyPoints**
[`string`](../../../data-types.md) | Number of story points. 

Data type is a string, as story points may not necessarily be a number ||
|| **epicId**
[`integer`](../../../data-types.md) | Identifier of the epic ||
|| **sort**
[`integer`](../../../data-types.md) | Sorting ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user who created the task ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user who last modified the task ||
|| **time**
[`array`](../../../data-types.md#time) | Information about the time taken for the request ||
|#

## Error Handling

HTTP Status: **200**

```json
{
    "error": 0,
    "error_description": "Task not found"
}
```

### Possible Error Codes

#|
|| **Code** | **Description**  | **Value** ||
|| `0` | Task not found | The task does not exist or the user does not have access to this task ||
|| `100` | Could not find value for parameter {id} | The parameter name is incorrect or the parameter is not set ||
|| `100` | Invalid value {stringValue} to match with parameter {id}. Should be value of type int. | Invalid parameter type ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./tasks-api-scrum-task-update.md)
- [{#T}](./tasks-api-scrum-task-get-fields.md)