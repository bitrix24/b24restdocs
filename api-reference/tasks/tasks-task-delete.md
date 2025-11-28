# Delete Task tasks.task.delete

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user with permission to delete a task

The method `tasks.task.delete` removes a task.

You can check the permission to delete a task using the [check access to task](./tasks-task-get-access.md) method.

{% note info "" %}

Since version `tasks 25.700.0`, the method call is available in two API versions.

Old version:

`https://{installation_address}/rest/{user_id}/{rest_application_password}/tasks.task.delete`

New version:

`https://{installation_address}/rest/api/{user_id}/{rest_application_password}/tasks.task.delete`

Documentation in OpenApi format is available for the new version of the method call. To obtain OpenApi, call the `documentation` method:

`https://{installation_address}/rest/api/{user_id}/{rest_application_password}/documentation`

{% endnote %}

## Method Parameters

{% include [Note on parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **taskId***
[`integer`](../data-types.md) | Identifier of the task.

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or using the [getting the task list](./tasks-task-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8131}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"taskId":8131,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.delete
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.delete',
            {
                taskId: 8131,
            }
        );
        
        const result = response.getData().result;
        console.log('Deleted task with ID:', result);
        
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
                'tasks.task.delete',
                [
                    'taskId' => 8131
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting task: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.task.delete',
        {
            'taskId': 8131
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
        'tasks.task.delete',
        [
            'taskId' => 8131
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
    "result": {
        "task": true
    },
    "time": {
        "start": 1758184832.67041,
        "finish": 1758184832.961555,
        "duration": 0.29114508628845215,
        "processing": 0.2604410648345947,
        "date_start": "2025-09-18T11:40:32+03:00",
        "date_finish": "2025-09-18T11:40:32+03:00",
        "operating_reset_at": 1758185432,
        "operating": 0.2604219913482666
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Root element of the response ||
|| **task**
[`boolean`](../data-types.md) | Returns `true` if the task was successfully deleted ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"1048582",
    "error_description":"No access to delete the task"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | wrong task id | The value of `taskId` is of an incorrect type ||
|| `1048582` | No access to delete the task | The user does not have access to the task or does not have permission to delete the task ||
|| `100` | CTaskItem All parameters in the constructor must have real class type | The required parameter `taskId` is missing ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-task-add.md)
- [{#T}](./tasks-task-update.md)
- [{#T}](./tasks-task-get.md)
- [{#T}](./tasks-task-list.md)
- [{#T}](./tasks-task-get-fields.md)