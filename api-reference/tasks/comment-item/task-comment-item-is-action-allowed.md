# Check if action is allowed for comment task.commentitem.isactionallowed

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.commentitem.isactionallowed` checks if an action is allowed for a comment.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TASKID***
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or by using the [method to get the list of tasks](../tasks-task-list.md) ||
|| **ITEMID***
[`integer`](../../data-types.md) | Comment identifier.

The comment identifier can be obtained when [adding a new comment](./task-comment-item-add.md) or by using the [method to get the list of comments](./task-comment-item-get-list.md) ||
|| **ACTIONID***
[`integer`](../../data-types.md) | Identifier of the action being checked:
- `1` — add comment 
- `2` — update comment 
- `3` — delete comment ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"ITEMID":3157,"ACTIONID":2}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.commentitem.isactionallowed
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"ITEMID":3157,"ACTIONID":2,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.commentitem.isactionallowed
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'task.commentitem.isactionallowed',
    		{
    			"TASKID": 8017,
    			"ITEMID": 3157,
    			"ACTIONID": 2
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    	console.log(result);
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
                'task.commentitem.isactionallowed',
                [
                    'TASKID'   => 8017,
                    'ITEMID'   => 3157,
                    'ACTIONID' => 2,
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
        echo 'Error checking if action is allowed: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.commentitem.isactionallowed',
        {
            "TASKID": 8017,
            "ITEMID": 3157,
            "ACTIONID": 2
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.commentitem.isactionallowed',
        [
            'TASKID' => 8017,
            'ITEMID' => 3157,
            'ACTIONID' => 2
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
        "start": 1753275597.386571,
        "finish": 1753275597.466896,
        "duration": 0.08032512664794922,
        "processing": 0.02973794937133789,
        "date_start": "2025-07-23T15:59:57+02:00",
        "date_finish": "2025-07-23T15:59:57+02:00",
        "operating_reset_at": 1753276197,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Result of the action permission check:
- `true` — allowed
- `false` — not allowed

Also returns `false` if non-existent identifiers are specified in the parameters. For example, if a task with `ID` = `95623` does not exist in the system ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_CORE",
    "error_description":"TASKS_ERROR_EXCEPTION_#256; Param #2 (actionId) expected by method ctaskcommentitem::isactionallowed(), but not given.; 256/TE/WRONG_ARGUMENTS.<br>"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #2 (actionId) expected by method ctaskcommentitem::isactionallowed(), but not given.; 256/TE/WRONG_ARGUMENTS | Required parameter not specified, for example, `ACTIONID` ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskcommentitem::isactionallowed() expected to be of type "integer", but given something else.; 256/TE/WRONG_ARGUMENTS | Incorrect value type specified for the parameter, for example, for `TASKID` ||
|| `ERROR_CORE` | TASKS_ERROR_ASSERT_EXCEPTION | The specified comment or task does not exist ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-comment-item-add.md)
- [{#T}](./task-comment-item-update.md)
- [{#T}](./task-comment-item-get.md)
- [{#T}](./task-comment-item-get-list.md)
- [{#T}](./task-comment-item-delete.md)
- [{#T}](./task-comment-item-get-manifest.md)