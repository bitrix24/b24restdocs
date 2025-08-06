# Get Comment by ID task.commentitem.get

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user with read access permission for the task or higher

The method `task.commentitem.get` retrieves a comment by its ID.

## Method Parameters

{% note warning "" %}

Pass parameters in the request according to the order in the table. If the order is violated, the request will return an error.

{% endnote %}

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TASKID***
[`integer`](../../data-types.md) | Task ID.

The task ID can be obtained when [creating a new task](../tasks-task-add.md) or by using the [get task list method](../tasks-task-list.md) ||
|| **ITEMID***
[`integer`](../../data-types.md) | Comment ID.

The comment ID can be obtained when [adding a new comment](./task-comment-item-add.md) or by using the [get comment list method](./task-comment-item-get-list.md) ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"ITEMID":3157}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.commentitem.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"ITEMID":3157,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.commentitem.get
    ```

- JS

    ```js
    BX24.callMethod(
        'task.commentitem.get',
        {
            "TASKID": 8017,
            "ITEMID": 3157
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
        'task.commentitem.get',
        [
            'TASKID' => 8017,
            'ITEMID' => 3157
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
        "POST_MESSAGE_HTML": null,
        "ID": "3157",
        "AUTHOR_ID": "503",
        "AUTHOR_NAME": "John Smith",
        "AUTHOR_EMAIL": "",
        "POST_DATE": "2025-07-15T14:30:00+02:00",
        "POST_MESSAGE": "Text of the new comment for the task",
        "ATTACHED_OBJECTS": {
            "973": {
                "ATTACHMENT_ID": "973",
                "NAME": "photo1.png",
                "SIZE": "1495700",
                "FILE_ID": "4755",
                "DOWNLOAD_URL": "/bitrix/tools/disk/uf.php?attachedId=973&auth%5Bauth%5D=3edf7ca92&action=download&ncc=1",
                "VIEW_URL": "/bitrix/tools/disk/uf.php?attachedId=973&auth%5Bauth%5D=3edf7ca92&action=show&ncc=1"
            },
            "975": {
                "ATTACHMENT_ID": "975",
                "NAME": "photo2.png",
                "SIZE": "1017053",
                "FILE_ID": "4753",
                "DOWNLOAD_URL": "/bitrix/tools/disk/uf.php?attachedId=975&auth%5Bauth%5D=3edf7ca92&action=download&ncc=1",
                "VIEW_URL": "/bitrix/tools/disk/uf.php?attachedId=975&auth%5Bauth%5D=3edf7ca92&action=show&ncc=1"
            }
        }
    },
    "time": {
        "start": 1753274863.280788,
        "finish": 1753274863.362892,
        "duration": 0.08210396766662598,
        "processing": 0.04009890556335449,
        "date_start": "2025-07-23T15:47:43+02:00",
        "date_finish": "2025-07-23T15:47:43+02:00",
        "operating_reset_at": 1753275463,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object describing the comment ||
|| **POST_MESSAGE_HTML**
[`string`](../../data-types.md) | HTML code of the comment ||
|| **ID**
[`string`](../../data-types.md) | Comment ID ||
|| **AUTHOR_ID**
[`string`](../../data-types.md) | Author ID of the comment ||
|| **AUTHOR_NAME**
[`string`](../../data-types.md) | Author name of the comment ||
|| **AUTHOR_EMAIL**
[`string`](../../data-types.md) | Author email of the comment ||
|| **POST_DATE**
[`string`](../../data-types.md) | Date and time of comment creation ||
|| **POST_MESSAGE**
[`string`](../../data-types.md) | Text of the comment ||
|| **ATTACHED_OBJECTS**
[`object`](../../data-types.md) | Object containing information about attachments. The key of the object is the attachment ID, and the value is the object with [file description](#attached-objects) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### ATTACHED_OBJECTS Object {#attached-objects}

#|
|| **Name**
`type` | **Description** ||
|| **ATTACHMENT_ID**
[`string`](../../data-types.md) | Attachment ID ||
|| **NAME**
[`string`](../../data-types.md) | File name ||
|| **SIZE**
[`string`](../../data-types.md) | File size in bytes ||
|| **FILE_ID**
[`string`](../../data-types.md) | File ID on Drive ||
|| **DOWNLOAD_URL**
[`string`](../../data-types.md) | URL to download the file ||
|| **VIEW_URL**
[`string`](../../data-types.md) | URL to view the file ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_CODE",
    "error_description":"TASKS_ERROR_EXCEPTION_#512; Check listitem not found or not accessible; 512/TE/ITEM_NOT_FOUND_OR_NOT_ACCESSIBLE.<br>"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#512; Check listitem not found or not accessible; 512/TE/ITEM_NOT_FOUND_OR_NOT_ACCESSIBLE | The error is returned in the following cases:
- Incorrect order of parameters in the method
- Task or comment with the specified ID not found
- No access permission to the task ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskcommentitem::get() expected to be of type "integer", but given something else.; 256/TE/WRONG_ARGUMENTS | An incorrect type of value was provided for the parameter, for example, for `TASKID` ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #1 (itemId) expected by method ctaskcommentitem::get(), but not given.; 256/TE/WRONG_ARGUMENTS | A required parameter was not provided, for example, `ITEMID` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-comment-item-add.md)
- [{#T}](./task-comment-item-update.md)
- [{#T}](./task-comment-item-get-list.md)
- [{#T}](./task-comment-item-delete.md)
- [{#T}](../../../tutorials/tasks/how-to-create-comment-with-file.md)