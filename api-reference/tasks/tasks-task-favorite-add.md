# Add Task to Favorites tasks.task.favorite.add

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user with read access to the task

The method `tasks.task.favorite.add` adds a task to Favorites.

## Method Parameters

{% include [Note on parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **taskId***
[`integer`](../data-types.md) | The identifier of the task.

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by using the [method to get the list of tasks](./tasks-task-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":119}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.favorite.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":119,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.favorite.add
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.favorite.add',
            {
                taskId: 119,
            }
        );
        
        const result = response.getData().result;
        console.log('Favorite added:', result);
        
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
                'tasks.task.favorite.add',
                [
                    'taskId' => 119
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
        'tasks.task.favorite.add',
        {
            'taskId': 119
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
        'tasks.task.favorite.add',
        [
            'taskId' => 119
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
        "start": 1758015040.372338,
        "finish": 1758015040.421553,
        "duration": 0.049214839935302734,
        "processing": 0.015886783599853516,
        "date_start": "2025-09-16T12:30:40+02:00",
        "date_finish": "2025-09-16T12:30:40+02:00",
        "operating_reset_at": 1758015640,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Returns `true` if the task was successfully added to favorites ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"1",
    "error_description":"Task not found or not accessible"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | wrong task id | The `taskId` parameter contains a value of an incorrect type ||
|| `1` | Task not found or not accessible | The task was not found or the user does not have access to the task ||
|| `100` | CTaskItem All parameters in the constructor must have real class type | The required parameter `taskId` is missing ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./tasks-task-favorite-remove.md)