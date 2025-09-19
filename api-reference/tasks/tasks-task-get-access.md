# Check Access to Task tasks.task.getaccess

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.getaccess` checks the available actions for users on a task.

## Method Parameters

{% include [Footnote on parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **taskId***
[`integer`](../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by using the [get task list](./tasks-task-list.md) method. ||
|| **users**
[`array`](../data-types.md) | Array of user identifiers for whom access needs to be checked.

By default, the current user is used.

User identifiers can be obtained using the [get user list](../user/user-get.md) method. ||
|#

## Code Examples

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8017,"users":[503,547]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.getaccess
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8017,"users":[503,547],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.getaccess
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.getaccess',
            {
                taskId: 8017,
                users: [503, 547],
            }
        );
        
        const result = response.getData().result;
        console.log('Access data:', result);
        
        processResult(result);
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
                'tasks.task.getaccess',
                [
                    'taskId' => 8017,
                    'users' => [503, 547]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.task.getaccess',
        {
            'taskId': 8017,
            'users': [503, 547]
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
        'tasks.task.getaccess',
        [
            'taskId' => 8017,
            'users' => [503, 547]
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
        "allowedActions": {
            "503": {
                "ACCEPT": false,
                "DECLINE": false,
                "COMPLETE": true,
                "APPROVE": false,
                "DISAPPROVE": false,
                "START": false,
                "PAUSE": true,
                "DELEGATE": true,
                "REMOVE": true,
                "EDIT": true,
                "DEFER": false,
                "RENEW": false,
                "CREATE": true,
                "CHANGE_DEADLINE": true,
                "CHECKLIST_ADD_ITEMS": true,
                "ADD_FAVORITE": false,
                "DELETE_FAVORITE": true,
                "RATE": true,
                "TAKE": false,
                "EDIT.ORIGINATOR": false,
                "CHECKLIST.REORDER": true,
                "ELAPSEDTIME.ADD": true,
                "DAYPLAN.TIMER.TOGGLE": true,
                "EDIT.PLAN": true,
                "CHECKLIST.ADD": true,
                "FAVORITE.ADD": false,
                "FAVORITE.DELETE": true
            },
            "547": {
                "ACCEPT": false,
                "DECLINE": false,
                "COMPLETE": false,
                "APPROVE": false,
                "DISAPPROVE": false,
                // ...
                "FAVORITE.DELETE": false
            }
        }
    },
    "time": {
        "start": 1758177122.815386,
        "finish": 1758177122.911002,
        "duration": 0.09561586380004883,
        "processing": 0.054609060287475586,
        "date_start": "2025-09-18T09:32:02+02:00",
        "date_finish": "2025-09-18T09:32:02+02:00",
        "operating_reset_at": 1758177722,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response.

Contains an object with a description of available actions for each user. ||
|| **allowedActions**
[`object`](../data-types.md) | An object where the key is the `user ID`, and the value is an object with [description of available actions](./fields.md#action) on the task.

If the user executing the method does not have access to the task, an empty array `"allowedActions":[]` will be returned.

{% note info "" %}

For non-existent users from the `users` parameter, the method will return a response with a value of `false` for all actions.

{% endnote %}

 ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"100",
    "error_description":"Invalid value {} to match with parameter {users}. Should be value of type array."
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | wrong task id | The value in the `taskId` parameter is of an incorrect type. ||
|| `100` | Invalid value {} to match with parameter {users}. Should be value of type array. | An incorrect value is specified in the `users` parameter. ||
|| `100` | CTaskItem All parameters in the constructor must have real class type | The required parameter `taskId` is not specified. ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)