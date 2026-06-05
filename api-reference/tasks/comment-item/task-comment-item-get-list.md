# Get the List of Comments task.commentitem.getlist

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user with read access permission for the task or higher.

The method `task.commentitem.getlist` retrieves a list of task comments.

{% note warning "DEPRECATED" %}

Development of this method has been halted since version `tasks 25.700.0`. The method task.commentitem.getlist does not work in the [new task card](../tasks-new.md); use the [im.dialog.messages.get](../../chats/messages/im-dialog-messages-get.md) method to work with task chat.

{% endnote %}

## Method Parameters

{% note warning "" %}

Pass parameters in the request according to the order in the table. If the order is violated, the request will return an error.

{% endnote %}

{% include [Parameter Notes](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **TASKID*** 
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or by using the [get task list](../tasks-task-list.md) method. ||
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

By default, the result is sorted in descending order by comment identifier. ||
|| **FILTER**
[`object`](../../data-types.md) | An object for filtering the result in the form `{"field": "filter value", ... }`. The value of the filtered field can be a single value or an array of values.

You can filter by the following fields: 
- `ID` — comment identifier
- `AUTHOR_ID` — comment author's identifier
- `AUTHOR_NAME` — author's name
- `POST_DATE` — comment publication date

You can specify a prefix for the filtered field indicating the type of filtering:
- `!` — not equal
- `<=` — less than or equal to
- `<` — less than
- `>=` — greater than or equal to
- `>` — greater than
 
By default, records are not filtered. ||
|#

## Code Examples

{% include [Example Notes](../../../_includes/examples.md) %}

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each comment item returned in the result array
    type TaskCommentItem = {
      ID: string
      AUTHOR_ID: string
      AUTHOR_NAME: string
      AUTHOR_EMAIL: string
      POST_DATE: ISODate | null
      POST_MESSAGE: string
      POST_MESSAGE_HTML: string | null
      ATTACHED_OBJECTS: Record<string, {
        ATTACHMENT_ID: string
        NAME: string
        SIZE: string
        FILE_ID: string
        DOWNLOAD_URL: string
        VIEW_URL: string
      }>
    }

    try {
      // task.commentitem.getlist returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<TaskCommentItem[]>({
        method: 'task.commentitem.getlist',
        params: {
          TASKID: 8017,
          ORDER: {
            POST_DATE: 'asc',
          },
          FILTER: {
            AUTHOR_ID: 503,
            '>=POST_DATE': '2025-01-01',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Comments fetched:', result.length, result)
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
      async function getTaskCommentList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // task.commentitem.getlist returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'task.commentitem.getlist',
            params: {
              TASKID: 8017,
              ORDER: {
                POST_DATE: 'asc',
              },
              FILTER: {
                AUTHOR_ID: 503,
                '>=POST_DATE': '2025-01-01',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Comments fetched:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTaskCommentList)
    </script>
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
        // Your logic for processing the data
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

HTTP Status: **200**

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
[`array`](../../data-types.md) | An array of objects. Each object contains a description of the comment. ||
|| **POST_MESSAGE_HTML**
[`string`](../../data-types.md) | HTML code of the comment. ||
|| **ID**
[`string`](../../data-types.md) | Comment identifier. ||
|| **AUTHOR_ID**
[`string`](../../data-types.md) | Comment author's identifier. ||
|| **AUTHOR_NAME**
[`string`](../../data-types.md) | Comment author's name. ||
|| **AUTHOR_EMAIL**
[`string`](../../data-types.md) | Comment author's email. ||
|| **POST_DATE**
[`string`](../../data-types.md) | Date and time of comment creation. ||
|| **POST_MESSAGE**
[`string`](../../data-types.md) | Text of the comment. ||
|| **ATTACHED_OBJECTS**
[`object`](../../data-types.md) | An object containing information about attachments. The key of the object is the attachment identifier, and the value is an object with [file description](#attached-objects). ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

### ATTACHED_OBJECTS Object {#attached-objects}

#| 
|| **Name**
`type` | **Description** ||
|| **ATTACHMENT_ID**
[`string`](../../data-types.md) | Attachment identifier. ||
|| **NAME**
[`string`](../../data-types.md) | File name. ||
|| **SIZE**
[`string`](../../data-types.md) | File size in bytes. ||
|| **FILE_ID**
[`string`](../../data-types.md) | File identifier on Drive. ||
|| **DOWNLOAD_URL**
[`string`](../../data-types.md) | URL for downloading the file. ||
|| **VIEW_URL**
[`string`](../../data-types.md) | URL for viewing the file. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_CORE",
    "error_description":"TASKS_ERROR_EXCEPTION_#256; Param #1 (arOrder) for method ctaskcommentitem::getlist() must not contain key ">=POST_DATE".; 256/TE/WRONG_ARGUMENTS.<br>"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#| 
|| **Code** | **Description**  | **Value** ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#8; Action failed; 8/TE/ACTION_FAILED_TO_BE_PROCESSED | Invalid parameter value specified or no access permission to the task. ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskcommentitem::getlist() expected to be of type "integer", but given something else.; 256/TE/WRONG_ARGUMENTS | Invalid type of value specified for the parameter, for example, for `TASKID`. ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #1 (arOrder) for method ctaskcommentitem::getlist() must not contain key ">=POST_DATE".; 256/TE/WRONG_ARGUMENTS | Parameters are specified in the wrong order. ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #2 (arFilter) for method ctaskcommentitem::getlist() must not contain key "%POST_DATE".; 256/TE/WRONG_ARGUMENTS | Incorrect parameter name or prefix specified for filtering. ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-comment-item-add.md)
- [{#T}](./task-comment-item-update.md)
- [{#T}](./task-comment-item-get.md)
- [{#T}](./task-comment-item-delete.md)