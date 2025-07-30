# Update Comment task.commentitem.update

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.commentitem.update` updates a comment.

## Method Parameters

{% note warning "" %}

Pass parameters in the request according to the order in the table. If the order is violated, the request will return an error.

{% endnote %}

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TASKID***
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or by using the [getting the task list method](../tasks-task-list.md) ||
|| **ITEMID***
[`integer`](../../data-types.md) | Comment identifier.

The comment identifier can be obtained when [adding a new comment](./task-comment-item-add.md) or by using the [getting the comment list method](./task-comment-item-get-list.md) ||
|| **FIELDS***
[`object`](../../data-types.md) | Object with [comment fields](#fields) ||
|#

### FIELDS Parameter {#fields}

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **POST_MESSAGE***
[`string`](../../data-types.md) | Message text ||
|| **AUTHOR_ID**
[`integer`](../../data-types.md) | Identifier of the user on whose behalf the comment is created.

You can get the user identifier using the [user.get](../../user/user-get.md) method ||
|| **POST_DATE**
[`string`](../../data-types.md) | Message date ||
|| **UF_FORUM_MESSAGE_DOC**
[`array`](../../data-types.md) | Array with file identifiers from Drive. Prefix each identifier with `n`, for example, `['n123', 'n456', ... ]`.

The comment author must have access to the attached files; otherwise, the method will return an error ||
|#

## Code Examples

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"ITEMID":3167,"FIELDS":{"POST_MESSAGE":"Comment updated","AUTHOR_ID":547,"UF_FORUM_MESSAGE_DOC":["n4755"]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.comm

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"ITEMID":3167,"FIELDS":{"POST_MESSAGE":"Comment updated","AUTHOR_ID":547,"UF_FORUM_MESSAGE_DOC":["n4755"]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.commentitem.update
    ```

- JS

    ```js
    BX24.callMethod(
        'task.commentitem.update',
        {
            "TASKID": 8017,
            "ITEMID": 3167,
            "FIELDS": {
                "POST_MESSAGE": "Comment updated",
                "AUTHOR_ID": 547,
                "UF_FORUM_MESSAGE_DOC": ["n4755"]
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
        'task.commentitem.update',
        [
            'TASKID' => 8017,
            'ITEMID' => 3167,
            'FIELDS' => [
                'POST_MESSAGE' => 'Comment updated',
                'AUTHOR_ID' => 547,
                'UF_FORUM_MESSAGE_DOC' => ['n4755']
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
    "result": true,
    "time": {
        "start": 1753338761.030934,
        "finish": 1753338761.389114,
        "duration": 0.35817980766296387,
        "processing": 0.16595101356506348,
        "date_start": "2025-07-24T09:32:41+02:00",
        "date_finish": "2025-07-24T09:32:41+02:00",
        "operating_reset_at": 1753339361,
        "operating": 0.16593098640441895
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the comment was successfully updated ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_CODE",
    "error_description":"Comment text is not specified.<br>"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_CORE` | Comment text is not specified. | Required parameter `POST_MESSAGE` is not provided or is empty ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#4; Action is not allowed; 4/TE/ACTION_NOT_ALLOWED | Error is returned in the following cases:
- Incorrect parameter order in the method
- No access permission to the task
- Attempting to update another user's comment
- If the specified task or comment does not exist ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskcommentitem::delete() expected to be of type "integer", but given something else.; 256/TE/WRONG_ARGUMENTS | Error is returned in the following cases:
- Required parameter, such as `TASKID`, is not specified
- Incorrect value type for the parameter, for example, for `TASKID` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-comment-item-add.md)
- [{#T}](./task-comment-item-get.md)
- [{#T}](./task-comment-item-get-list.md)
- [{#T}](./task-comment-item-delete.md)
- [{#T}](./task-comment-item-is-action-allowed.md)
- [{#T}](./task-comment-item-get-manifest.md)