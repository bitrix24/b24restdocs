# Add Comment task.commentitem.add

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.commentitem.add` adds a new comment to a task.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TASKID***
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or by using the [get task list](../tasks-task-list.md) method ||
|| **FIELDS***
[`object`](../../data-types.md) | An object with [comment fields](#fields) ||
|#

### FIELDS Parameter {#fields}

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **POST_MESSAGE***
[`string`](../../data-types.md) | Message text ||
|| **AUTHOR_ID**
[`integer`](../../data-types.md) | Identifier of the user on behalf of whom the comment should be created.

You can obtain the user identifier using the [user.get](../../user/user-get.md) method ||
|| **POST_DATE**
[`string`](../../data-types.md) | Message date ||
|| **UF_FORUM_MESSAGE_DOC**
[`array`](../../data-types.md) | An array of file identifiers from Drive. Prefix each identifier with `n`, for example, `['n123', 'n456', ... ]`.

The comment author must have access to the attached files; otherwise, the method will return an error ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"FIELDS":{"POST_MESSAGE":"Text of the new comment for the task","AUTHOR_ID":503,"POST_DATE":"2025-07-15T14:30:00+02:00","UF_FORUM_MESSAGE_DOC":["n4755","n4753"]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.commentitem.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"FIELDS":{"POST_MESSAGE":"Text of the new comment for the task","AUTHOR_ID":503,"POST_DATE":"2025-07-15T14:30:00+02:00","UF_FORUM_MESSAGE_DOC":["n4755","n4753"]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.commentitem.add
    ```

- JS

    ```js
    BX24.callMethod(
        'task.commentitem.add',
        {
            "TASKID": 8017,
            "FIELDS": {
                "POST_MESSAGE": "Text of the new comment for the task",
                "AUTHOR_ID": 503,
                "POST_DATE": "2025-07-15T14:30:00+02:00",
                "UF_FORUM_MESSAGE_DOC": ["n4755", "n4753"]
            }
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.commentitem.add',
        [
            'TASKID' => 8017,
            'FIELDS' => [
                'POST_MESSAGE' => 'Text of the new comment for the task',
                'AUTHOR_ID' => 503,
                'POST_DATE' => '2025-07-15T14:30:00+02:00',
                'UF_FORUM_MESSAGE_DOC' => ['n4755', 'n4753']
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
    "result": 3141,
    "time": {
        "start": 1753262861.683775,
        "finish": 1753262862.001611,
        "duration": 0.31783604621887207,
        "processing": 0.27428317070007324,
        "date_start": "2025-07-23T12:27:41+02:00",
        "date_finish": "2025-07-23T12:27:42+02:00",
        "operating_reset_at": 1753263461,
        "operating": 0.2742629051208496
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Identifier of the new comment ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#


## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_CODE",
    "error_description":"Insufficient permissions to add a comment.<br>"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value**  ||
|| `ERROR_CORE` | Comment text not specified | Required parameter `POST_MESSAGE` not provided or is empty ||
|| `ERROR_CORE` | Insufficient permissions to add a comment | No access permission to the task ||
|| `ERROR_CORE` | File not found | File from the `UF_FORUM_MESSAGE_DOC` parameter not found or the author does not have access to it ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #1 (arFields) for method ctaskcommentitem::add() must not contain key `<FIELD_NAME>`.; 256/TE/WRONG_ARGUMENTS | Field `<FIELD_NAME>` cannot be used in the method ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskcommentitem::add() expected to be of type "integer", but given something else.; 256/TE/WRONG_ARGUMENTS | Incorrect value type for the parameter, for example, for `TASKID` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-comment-item-update.md)
- [{#T}](./task-comment-item-get.md)
- [{#T}](./task-comment-item-get-list.md)
- [{#T}](./task-comment-item-delete.md)
- [{#T}](./task-comment-item-is-action-allowed.md)
- [{#T}](./task-comment-item-get-manifest.md)
- [{#T}](../../../tutorials/tasks/how-to-create-comment-with-file.md)