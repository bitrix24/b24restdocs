# Update Time Entry task.elapseditem.update

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates the parameters of the specified time entry.

{% note info %}

You can check the permission to modify using the special method [task.elapseditem.isactionallowed](./task-elapsed-item-is-action-allowed.md)

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TASKID***
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or by using the [get task list method](../tasks-task-list.md) ||
|| **ITEMID***
[`integer`](../../data-types.md) | Time entry identifier.

It can be obtained when [creating a new entry](./task-elapsed-item-add.md) or by using the [get time entry list method](./task-elapsed-item-get-list.md) ||
|| **ARFIELDS***
[`object`](../../data-types.md) | An object containing user records, time, and comments (detailed description provided below) in the following structure:

```js
"ARFIELDS": {
    "SECONDS": "value", 
    "COMMENT_TEXT": "value",
    "USER_ID": "value"
},
```

 ||
|#

### ARFIELDS Parameter

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **SECONDS***
[`integer`](../../data-types.md) | Amount of time spent in seconds ||
|| **COMMENT_TEXT***
[`string`](../../data-types.md) | Comment text ||
|| **USER_ID**
[`integer`](../../data-types.md) | User identifier ||
|#

{% note warning %}

It is mandatory to follow the specified order of parameters in the request as shown in the tables. Otherwise, the request will be executed with errors.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID" : 691, "ITEMID": 5, "ARFIELDS": {"SECONDS": 113,"COMMENT_TEXT": "comment text"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.elapseditem.update
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID" : 691, "ITEMID": 5, "ARFIELDS": {"SECONDS": 113,"COMMENT_TEXT": "comment text"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.elapseditem.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'task.elapseditem.update',
    		{
    			"TASKID": 691,
    			"ITEMID": 5,
    			"ARFIELDS": {
    				"SECONDS": 113, 
    				"COMMENT_TEXT": "comment text",
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
                'task.elapseditem.update',
                [
                    'TASKID'   => 691,
                    'ITEMID'   => 5,
                    'ARFIELDS' => [
                        'SECONDS'      => 113,
                        'COMMENT_TEXT' => 'comment text',
                    ],
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
        echo 'Error updating elapsed item: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.elapseditem.update',
        {
            "TASKID": 691,
            "ITEMID": 5,
            "ARFIELDS": {
                "SECONDS": 113, 
                "COMMENT_TEXT": "comment text",
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
        'task.elapseditem.update',
        [
            'TASKID' => 691,
            'ITEMID' => 5,
            'ARFIELDS' => [
                'SECONDS' => 113,
                'COMMENT_TEXT' => 'comment text',
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

In case of a successful request, the server will return `result:null`

```json
{
    "result": null,
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
    "error": "ERROR_CORE",
    "error_description": "ACTION_NOT_ALLOWED"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `0x000001` | Task not found ||
|| `0x100002` | Access denied ||
|| `0x000004` | Action not allowed ||
|| `0x000040` | Unknown error ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-elapsed-item-add.md)
- [{#T}](./task-elapsed-item-get.md)
- [{#T}](./task-elapsed-item-get-list.md)
- [{#T}](./task-elapsed-item-delete.md)
- [{#T}](./task-elapsed-item-is-action-allowed.md)
- [{#T}](./task-elapsed-item-get-manifest.md)