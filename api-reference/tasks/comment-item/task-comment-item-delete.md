# Delete Comment task.commentitem.delete

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `task.commentitem.delete` removes a comment.

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

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or by using the [method for retrieving the list of tasks](../tasks-task-list.md) ||
|| **ITEMID***
[`integer`](../../data-types.md) | Comment identifier.

The comment identifier can be obtained when [adding a new comment](./task-comment-item-add.md) or by using the [method for retrieving the list of comments](./task-comment-item-get-list.md) ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"ITEMID":3155}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.commentitem.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"ITEMID":3155,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.commentitem.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'task.commentitem.delete',
        {
            "TASKID": 8017,
            "ITEMID": 3155
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
        'task.commentitem.delete',
        [
            'TASKID' => 8017,
            'ITEMID' => 3155
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
    "result": true,
    "time": {
        "start": 1753274713.135909,
        "finish": 1753274713.503945,
        "duration": 0.36803603172302246,
        "processing": 0.32417798042297363,
        "date_start": "2025-07-23T15:45:13+02:00",
        "date_finish": "2025-07-23T15:45:13+02:00",
        "operating_reset_at": 1753275313,
        "operating": 0.32415318489074707
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the comment was successfully deleted ||
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
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #1 (itemId) expected by method ctaskcommentitem::delete(), but not given.; 256/TE/WRONG_ARGUMENTS | Required parameter not specified, for example, `ITEMID` ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#4; Action is not allowed; 4/TE/ACTION_NOT_ALLOWED | Error returned in several cases:
- Incorrect parameter order
- No access permission to the task
- Cannot delete another user's comment unless you are an administrator
- The specified task or comment does not exist ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskcommentitem::delete() expected to be of type "integer", but given something else.; 256/TE/WRONG_ARGUMENTS | Incorrect value type specified for the parameter, for example, for `TASKID` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-comment-item-add.md)
- [{#T}](./task-comment-item-update.md)
- [{#T}](./task-comment-item-get.md)
- [{#T}](./task-comment-item-get-list.md)