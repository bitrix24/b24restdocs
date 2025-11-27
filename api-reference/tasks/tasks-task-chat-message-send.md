# Send a message to the task chat tasks.task.chat.message.send

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user with read access permission for the task or higher

The method `tasks.task.chat.message.send` sends a new message to the task chat.

## Method Parameters

{% include [Footnote about parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../data-types.md) | Object with [message parameters](#fields) ||
|#

### Parameter fields {#fields}

{% include [Footnote about parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **taskId***
[`integer`](../data-types.md) | Identifier of the task to which the message should be sent. 

The task identifier can be obtained when [creating a new task](./tasks-task-add.md) or by using the [get task list method](./tasks-task-list.md) ||
|| **text***
[`string`](../data-types.md) | Message text ||
|#

## Code Examples

{% include [Footnote about examples](../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by the addition of the /api/ parameter in the request. 

Old version:

`https://{installation_address}/rest/{user_id}/{rest_application_password}/tasks.task.get`

New version:

`https://{installation_address}/rest/api/{user_id}/{rest_application_password}/tasks.task.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"taskId":51,"text":"Message from external system"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.chat.message.send
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"taskId":51,"text":"Message from external system"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.task.chat.message.send
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.chat.message.send',
            {
                fields: {
                    taskId: 51,
                    text: 'Message from external system',
                }
            }
        );
        
        const result = response.getData().result;
        console.log('Message sent with ID:', result);
        
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
                'tasks.task.chat.message.send',
                [
                    'fields' =>
                    [
                        'taskId' => 51,
                        'text' => 'Message from external system'
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error sending chat message: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.task.chat.message.send',
        {
            'fields': {
                'taskId': 51,
                'text': 'Message from external system'
            }
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
        'tasks.task.chat.message.send',
        [
            'fields' =>
            [
                'taskId' => 51,
                'text' => 'Message from external system'
            ]
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
        "result": true
    },
    "time": {
        "start": 1762257254,
        "finish": 1762257254.211513,
        "duration": 0.21151304244995117,
        "processing": 0,
        "date_start": "2025-11-04T15:54:14+02:00",
        "date_finish": "2025-11-04T15:54:14+02:00",
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

Contains an object with the key `result` and the value `true` if the message was sent successfully ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "Bad Request",
    "error_description": "Error during object validation"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** |**Code** | **Description** | **Value** ||
|| `400` | Bad Request | Error during object validation | Required field not provided or it is empty ||
|| `403` | No access to the task |  | User does not have access permission to the task ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-task-add.md)
- [{#T}](./tasks-task-update.md)