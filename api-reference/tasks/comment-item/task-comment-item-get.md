# Get Comment by ID task.commentitem.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user with read access permission to the task or higher.

The method `task.commentitem.get` retrieves a comment by its ID.

{% note warning "DEPRECATED" %}

Development of this method has been halted since version `tasks 25.700.0`. The method task.commentitem.get does not work in the [new task card](../tasks-new.md); use the [im.dialog.messages.get](../../chats/messages/im-dialog-messages-get.md) method to work with task chat.

{% endnote %}

## Method Parameters

{% note warning "" %}

Pass parameters in the request according to the order in the table. If the order is violated, the request will return an error.

{% endnote %}

{% include [Parameter Note](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **TASKID*** 
[`integer`](../../data-types.md) | Task ID.

The task ID can be obtained when [creating a new task](../tasks-task-add.md) or by using the [get task list](../tasks-task-list.md) method. ||
|| **ITEMID*** 
[`integer`](../../data-types.md) | Comment ID.

The comment ID can be obtained when [adding a new comment](./task-comment-item-add.md) or by using the [get comment list](./task-comment-item-get-list.md) method. ||
|#

## Code Examples

{% include [Example Note](../../../_includes/examples.md) %}

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type AttachedObject = {
      ATTACHMENT_ID: string
      NAME: string
      SIZE: string
      FILE_ID: string
      DOWNLOAD_URL: string
      VIEW_URL: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CommentItemGetResult = {
      POST_MESSAGE_HTML: string | null
      ID: string
      AUTHOR_ID: string
      AUTHOR_NAME: string
      AUTHOR_EMAIL: string
      POST_DATE: ISODate | null
      POST_MESSAGE: string
      ATTACHED_OBJECTS: Record<string, AttachedObject>
    }

    try {
      const response = await $b24.actions.v2.call.make<CommentItemGetResult>({
        method: 'task.commentitem.get',
        params: {
          TASKID: 8017,
          ITEMID: 3157,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.ID, result.POST_MESSAGE, result.POST_DATE)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getCommentItem() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.commentitem.get',
            params: {
              TASKID: 8017,
              ITEMID: 3157,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.ID, result.POST_MESSAGE, result.POST_DATE)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCommentItem)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.commentitem.get',
                [
                    'TASKID' => 8017,
                    'ITEMID' => 3157
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting task comments: ' . $e->getMessage();
    }
    ```

- BX24.js

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

- PHP CRest

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

HTTP Status: **200**

```json
{
    "result": {
        "POST_MESSAGE_HTML": null,
        "ID": "3157",
        "AUTHOR_ID": "503",
        "AUTHOR_NAME": "John Doe",
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
[`string`](../../data-types.md) | Comment author's ID ||
|| **AUTHOR_NAME**
[`string`](../../data-types.md) | Comment author's name ||
|| **AUTHOR_EMAIL**
[`string`](../../data-types.md) | Comment author's email ||
|| **POST_DATE**
[`string`](../../data-types.md) | Date and time the comment was created ||
|| **POST_MESSAGE**
[`string`](../../data-types.md) | Comment text ||
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
[`string`](../../data-types.md) | URL for downloading the file ||
|| **VIEW_URL**
[`string`](../../data-types.md) | URL for viewing the file ||
|#

## Error Handling

HTTP Status: **400**

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
- Incorrect parameter order in the method
- Task or comment with the specified ID not found
- No access permission to the task ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskcommentitem::get() expected to be of type "integer", but given something else.; 256/TE/WRONG_ARGUMENTS | Incorrect value type for the parameter, for example, for `TASKID` ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #1 (itemId) expected by method ctaskcommentitem::get(), but not given.; 256/TE/WRONG_ARGUMENTS | Required parameter not provided, for example, `ITEMID` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-comment-item-add.md)
- [{#T}](./task-comment-item-update.md)
- [{#T}](./task-comment-item-get-list.md)
- [{#T}](./task-comment-item-delete.md)
- [{#T}](../../../tutorials/tasks/how-to-create-comment-with-file.md)