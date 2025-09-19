# Add Comment as Task Result tasks.task.result.addFromComment

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user with access to the task

The method `tasks.task.result.addFromComment` attaches a comment as the result of a task.

A user can only attach their own comment as a result. An administrator can attach any user's comment, becoming the author of the result.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **commentId***
[`integer`](../../data-types.md) | The identifier of the comment to be attached as a result.

The comment identifier can be obtained when [adding a new comment](../comment-item/task-comment-item-add.md) or by using the [method to get the list of comments](../comment-item/task-comment-item-get-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"commentId":3199}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.result.addFromComment
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"commentId":3199,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.result.addFromComment
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.result.addFromComment',
            {
                commentId: 3199,
            }
        );
        
        const result = response.getData().result;
        console.log('Task result added from comment:', result);
        
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
                'tasks.task.result.addFromComment',
                [
                    'commentId' => 3199
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding task result from comment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.task.result.addFromComment',
        {
            "commentId": 3199
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
        'tasks.task.result.addFromComment',
        [
            'commentId' => 3199
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
        "id": 21,
        "taskId": 8017,
        "commentId": 3199,
        "createdBy": 503,
        "createdAt": "2025-07-13T14:30:00+02:00",
        "updatedAt": "2025-07-13T14:30:00+02:00",
        "status": 0,
        "text": "Sent documents to the client. The client promises to respond on [B]Monday[\/B].",
        "formattedText": "Sent documents to the client. The client promises to respond on \u003Cb\u003EMonday\u003C\/b\u003E.",
        "files": null
    },
    "time": {
        "start": 1755597246.027815,
        "finish": 1755597246.115861,
        "duration": 0.08804583549499512,
        "processing": 0.05956697463989258,
        "date_start": "2025-08-19T12:54:06+02:00",
        "date_finish": "2025-08-19T12:54:06+02:00",
        "operating_reset_at": 1755597846,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | An object describing the attached result ||
|| **id**
[`integer`](../../data-types.md) | The identifier of the result ||
|| **taskId**
[`integer`](../../data-types.md) | The identifier of the task ||
|| **commentId**
[`integer`](../../data-types.md) | The identifier of the comment attached as a result ||
|| **createdBy**
[`integer`](../../data-types.md) | The identifier of the user who attached the result ||
|| **createdAt**
[`string`](../../data-types.md) | The date and time the result was attached in ISO 8601 format ||
|| **updatedAt**
[`string`](../../data-types.md) | The date and time of the last update to the result in ISO 8601 format ||
|| **status**
[`integer`](../../data-types.md) | The status of the result. Possible values:
- `0` — result is open
- `1` — result is closed

The result becomes closed after the task is completed and retains this status after the task is resumed. Only new results in an unfinished task will be open.

A comment with an open result cannot be re-added to the result. If the result is closed, adding is possible
 ||
|| **text**
[`string`](../../data-types.md) | The text of the result ||
|| **formattedText**
[`string`](../../data-types.md) | The formatted text of the result ||
|| **files**
`null` | Has a value of `null`. 

The list of files attached to the result can be obtained using the method [tasks.task.result.list](./tasks-task-result-list.md) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"0",
    "error_description":"Comment not found."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Access denied. | The user does not have permission to access the task or the comment does not belong to the user ||
|| `0` | Result already exists. | The comment is already attached as a result ||
|| `100` | Invalid value {value} to match with parameter {commentId}. Should be value of type int. | An invalid type value was passed in the `commentId` parameter. It should be of type `integer` ||
|| `0` | Comment not found. | A comment with this identifier does not exist ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-task-result-list.md)
- [{#T}](./tasks-task-result-delete-from-comment.md)