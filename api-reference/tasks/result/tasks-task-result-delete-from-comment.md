# Delete Comment from Result tasks.task.result.deleteFromComment

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user with access to the task

The method `tasks.task.result.deleteFromComment` unpins a comment as the result of a task. To remove a comment from the result, use the method [task.commentitem.delete](../comment-item/task-comment-item-delete.md).

A user can only unpin their own comment. An administrator can unpin any user's comment.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **commentId***
[`integer`](../../data-types.md) | The identifier of the comment for which the result needs to be unpinned. 

The comment identifier can be obtained when [adding a new comment](../comment-item/task-comment-item-add.md) or by using the [method to get the list of comments](../comment-item/task-comment-item-get-list.md) for the task ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"commentId":3199}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.result.deleteFromComment
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"commentId":3199,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/tasks.task.result.deleteFromComment
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.result.deleteFromComment',
            {
                commentId: 3199,
            }
        );
        
        const result = response.getData().result;
        console.log('Deleted comment with ID:', result);
        
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
                'tasks.task.result.deleteFromComment',
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
        echo 'Error deleting comment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.task.result.deleteFromComment',
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
        'tasks.task.result.deleteFromComment',
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

HTTP Status: **200**

```json
{
    "result": null,
    "time": {
        "start": 1755611282.263157,
        "finish": 1755611282.322503,
        "duration": 0.05934619903564453,
        "processing": 0.031157970428466797,
        "date_start": "2025-08-19T16:48:02+02:00",
        "date_finish": "2025-08-19T16:48:02+02:00",
        "operating_reset_at": 1755611882,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
`null` | Returns `null` in case of successful execution ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_CORE",
    "error_description":"TASKS_ERROR_EXCEPTION_#4; Action is not allowed; 4/TE/ACTION_NOT_ALLOWED.<br>"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `0` | Access denied. | The user does not have permission to access the task or the comment does not belong to the user ||
|| `100` | Invalid value {value} to match with parameter {commentId}. Should be value of type int. | The value provided for the `commentId` parameter is of an invalid type. It should be of type `integer` ||
|| `0` | Comment not found. | A comment with such an identifier does not exist ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-task-result-list.md)
- [{#T}](./tasks-task-result-add-from-comment.md)