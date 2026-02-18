# Get the list of comments task.commentitem.getlist

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user with read access permission for the task or higher

The method `task.commentitem.getlist` retrieves a list of comments for a task.

{% note warning "Development of the method has been halted since version `tasks 25.700.0` " %}

The method `task.commentitem.getlist` does not work in the [new task card](../tasks-new.md), use the method [im.dialog.messages.get](../../chats/messages/im-dialog-messages-get.md) to work with task chat.

{% endnote %}

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

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or by using the [get task list method](../tasks-task-list.md) ||
|| **ORDER**
[`object`](../../data-types.md) | An object for sorting the result in the form `{"field": "sort value", ... }`.

You can sort by the following fields:
- `ID` — comment identifier 
- `AUTHOR_ID` — comment author's identifier 
- `AUTHOR_NAME` — author's name
- `AUTHOR_EMAIL` — author's email address
- `POST_DATE` — comment publication date 

The sorting direction can take the following values:
- `asc` — ascending
- `desc` — descending

By default, the result is sorted in descending order by comment identifier ||
|| **FILTER**
[`object`](../../data-types.md) | An object for filtering the result in the form `{"field": "filter value", ... }`. The value of the filtered field can be a single value or an array of values.

You can filter by the following fields: 
- `ID` — comment identifier
- `AUTHOR_ID` — comment author's identifier
- `AUTHOR_NAME` — author's name
- `POST_DATE` — comment publication date

You can specify a prefix with the type of filtering before the name of the filtered field:
- `!` — not equal
- `<=`— less than or equal to
- `<` — less than
- `>=`— greater than or equal to
- `>` — greater than
 
By default, records are not filtered ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"ORDER":{"POST_DATE":"asc"},"FILTER":{"AUTHOR_ID":503,">=POST_DATE":"2025-01-01"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.commentitem.getlist
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"ORDER":{"POST_DATE":"asc"},"FILTER":{"AUTHOR_ID":503,">=POST_DATE":"2025-01-01"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.commentitem.getlist
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'task.commentitem.getlist',
        {
          "TASKID": 8017,
          "ORDER": {
            "POST_DATE": "asc",
          },
          "FILTER": {
            "AUTHOR_ID": 503,
            ">=POST_DATE": "2025-01-01",
          }
        },
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('task.commentitem.getlist', {
        "TASKID": 8017,
        "ORDER": {
          "POST_DATE": "asc",
        },
        "FILTER": {
          "AUTHOR_ID": 503,
          ">=POST_DATE": "2025-01-01",
        }
      }, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('task.commentitem.getlist', {
        "TASKID": 8017,
        "ORDER": {
          "POST_DATE": "asc",
        },
        "FILTER": {
          "AUTHOR_ID": 503,
          ">=POST_DATE": "2025-01-01",
        }
      }, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.commentitem.getlist',
                [
                    'TASKID' => 8017,
                    'ORDER' => [
                        'POST_DATE' => 'asc',
                    ],
                    'FILTER' => [
                        'AUTHOR_ID' => 503,
                        '>=POST_DATE' => '2025-01-01',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting task comments: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.commentitem.getlist',
        {
            "TASKID": 8017,
            "ORDER": {
                "POST_DATE": "asc",
            },
            "FILTER": {
                "AUTHOR_ID": 503,
                ">=POST_DATE": "2025-01-01",
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
        'task.commentitem.getlist',
        [
            'TASKID' => 8017,
            'ORDER' => [
                'POST_DATE' => 'asc',
            ],
            'FILTER' => [
                'AUTHOR_ID' => 503,
                '>=POST_DATE' => '2025-01-01',
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
    "result": [
        {
            "POST_MESSAGE_HTML": null,
            "ID": "3157",
            "AUTHOR_ID": "503",
            "AUTHOR_NAME": "John Smith",
            "AUTHOR_EMAIL": "",
            "POST_DATE": "2025-07-15T14:31:00+02:00",
            "POST_MESSAGE": "Photos attached",
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
        {
            "POST_MESSAGE_HTML": null,
            "ID": "3155",
            "AUTHOR_ID": "503",
            "AUTHOR_NAME": "John Smith",
            "AUTHOR_EMAIL": "",
            "POST_DATE": "2025-07-15T14:30:00+02:00",
            "POST_MESSAGE": "Prepared new photos",
            "ATTACHED_OBJECTS": {}
        }
    ],
    "time": {
        "start": 1753270901.224447,
        "finish": 1753270901.343166,
        "duration": 0.11871910095214844,
        "processing": 0.06380701065063477,
        "date_start": "2025-07-23T14:41:41+02:00",
        "date_finish": "2025-07-23T14:41:41+02:00",
        "operating_reset_at": 1753271501,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | An array of objects. Each object contains a description of the comment ||
|| **POST_MESSAGE_HTML**
[`string`](../../data-types.md) | HTML code of the comment ||
|| **ID**
[`string`](../../data-types.md) | Comment identifier ||
|| **AUTHOR_ID**
[`string`](../../data-types.md) | Comment author's identifier ||
|| **AUTHOR_NAME**
[`string`](../../data-types.md) | Comment author's name ||
|| **AUTHOR_EMAIL**
[`string`](../../data-types.md) | Comment author's email ||
|| **POST_DATE**
[`string`](../../data-types.md) | Date and time of comment creation ||
|| **POST_MESSAGE**
[`string`](../../data-types.md) | Text of the comment ||
|| **ATTACHED_OBJECTS**
[`object`](../../data-types.md) | An object containing information about attachments. The key of the object is the attachment identifier, and the value is an object with [file description](#attached-objects) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### ATTACHED_OBJECTS Object {#attached-objects}

#|
|| **Name**
`type` | **Description** ||
|| **ATTACHMENT_ID**
[`string`](../../data-types.md) | Attachment identifier ||
|| **NAME**
[`string`](../../data-types.md) | File name ||
|| **SIZE**
[`string`](../../data-types.md) | File size in bytes ||
|| **FILE_ID**
[`string`](../../data-types.md) | File identifier on Drive ||
|| **DOWNLOAD_URL**
[`string`](../../data-types.md) | URL for downloading the file ||
|| **VIEW_URL**
[`string`](../../data-types.md) | URL for viewing the file ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"ERROR_CORE",
    "error_description":"TASKS_ERROR_EXCEPTION_#256; Param #1 (arOrder) for method ctaskcommentitem::getlist() must not contain key ">=POST_DATE".; 256/TE/WRONG_ARGUMENTS.<br>"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description**  | **Value** ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Action failed; 8/TE/ACTION_FAILED_TO_BE_PROCESSED | An incorrect parameter value is specified or there are no access permissions for the task ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskcommentitem::getlist() expected to be of type "integer", but given something else.; 256/TE/WRONG_ARGUMENTS | An incorrect type of value is specified for the parameter, for example, for `TASKID` ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #1 (arOrder) for method ctaskcommentitem::getlist() must not contain key ">=POST_DATE".; 256/TE/WRONG_ARGUMENTS | Parameters are specified in the wrong order ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #2 (arFilter) for method ctaskcommentitem::getlist() must not contain key "%POST_DATE".; 256/TE/WRONG_ARGUMENTS | The parameter name or prefix for filtering is incorrectly specified ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-comment-item-add.md)
- [{#T}](./task-comment-item-update.md)
- [{#T}](./task-comment-item-get.md)
- [{#T}](./task-comment-item-delete.md)

