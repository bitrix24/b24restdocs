# Add Comment task.commentitem.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user with read access permission to the task or higher.

The method `task.commentitem.add` adds a new comment to a task.

{% note warning "DEPRECATED" %}

The development of this method has been halted since version `tasks 25.700.0`. Use [tasks.task.chat.message.send](../../rest-v3/tasks/tasks-task-chat-message-send.md).

{% endnote %}

## Method Parameters

{% include [Parameter Note](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **TASKID*** 
[`integer`](../../data-types.md) | Task identifier.

The task identifier can be obtained when [creating a new task](../tasks-task-add.md) or by using the [method to get the list of tasks](../tasks-task-list.md) ||
|| **FIELDS*** 
[`object`](../../data-types.md) | Object with [comment fields](#fields) ||
|#

### FIELDS Parameter {#fields}

{% include [Parameter Note](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **POST_MESSAGE*** 
[`string`](../../data-types.md) | Message text ||
|| **AUTHOR_ID** 
[`integer`](../../data-types.md) | Identifier of the user on whose behalf the comment should be created.

You can obtain the user identifier using the [user.get](../../user/user-get.md) method.

{% note alert "" %}

The method `task.commentitem.add` allows any user to add a comment on behalf of another user.

{% endnote %}

 ||
|| **POST_DATE** 
[`string`](../../data-types.md) | Message date ||
|| **UF_FORUM_MESSAGE_DOC** 
[`array`](../../data-types.md) | Array with identifiers of files from Drive. Prefix each identifier with `n`, for example, `['n123', 'n456', ... ]`.

The comment author must have access to the attached files; otherwise, the method will return an error. ||
|#

## Code Examples

{% include [Example Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"FIELDS":{"POST_MESSAGE":"Text of the new task comment","AUTHOR_ID":503,"POST_DATE":"2025-07-15T14:30:00+02:00","UF_FORUM_MESSAGE_DOC":["n4755","n4753"]}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.commentitem.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"TASKID":8017,"FIELDS":{"POST_MESSAGE":"Text of the new task comment","AUTHOR_ID":503,"POST_DATE":"2025-07-15T14:30:00+02:00","UF_FORUM_MESSAGE_DOC":["n4755","n4753"]},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.commentitem.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (the ID of the newly created comment)
    type CommentItemAddResult = number

    try {
      const response = await $b24.actions.v2.call.make<CommentItemAddResult>({
        method: 'task.commentitem.add',
        params: {
          TASKID: 8017,
          FIELDS: {
            POST_MESSAGE: 'Text of the new task comment',
            AUTHOR_ID: 503,
            POST_DATE: '2025-07-15T14:30:00+02:00',
            UF_FORUM_MESSAGE_DOC: ['n4755', 'n4753'],
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('New comment ID:', result)
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
      async function addTaskComment() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'task.commentitem.add',
            params: {
              TASKID: 8017,
              FIELDS: {
                POST_MESSAGE: 'Text of the new task comment',
                AUTHOR_ID: 503,
                POST_DATE: '2025-07-15T14:30:00+02:00',
                UF_FORUM_MESSAGE_DOC: ['n4755', 'n4753'],
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
          console.info('New comment ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addTaskComment)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.commentitem.add',
                [
                    'TASKID' => 8017,
                    'FIELDS' => [
                        'POST_MESSAGE'         => 'Text of the new task comment',
                        'AUTHOR_ID'            => 503,
                        'POST_DATE'            => '2025-07-15T14:30:00+02:00',
                        'UF_FORUM_MESSAGE_DOC' => ['n4755', 'n4753'],
                    ],
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
        echo 'Error adding task comment: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.commentitem.add',
        {
            "TASKID": 8017,
            "FIELDS": {
                "POST_MESSAGE": "Text of the new task comment",
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.commentitem.add',
        [
            'TASKID' => 8017,
            'FIELDS' => [
                'POST_MESSAGE' => 'Text of the new task comment',
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

HTTP Status: **200**

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
[`integer`](../../data-types.md) | Identifier of the new comment ||
|| **time** 
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

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
|| `ERROR_CORE` | Comment text is not specified | Required parameter `POST_MESSAGE` is not provided or is empty ||
|| `ERROR_CORE` | Insufficient permissions to add a comment | No access permission to the task ||
|| `ERROR_CORE` | File not found | The file from the `UF_FORUM_MESSAGE_DOC` parameter is not found or the author does not have access to it ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #1 (arFields) for method ctaskcommentitem::add() must not contain key `<FIELD_NAME>`.; 256/TE/WRONG_ARGUMENTS | The field `<FIELD_NAME>` cannot be used in the method ||
|| `ERROR_CORE` | TASKS_ERROR_EXCEPTION_#256; Param #0 (taskId) for method ctaskcommentitem::add() expected to be of type "integer", but given something else.; 256/TE/WRONG_ARGUMENTS | An incorrect value type for the parameter was specified, for example, for `TASKID` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-comment-item-update.md)
- [{#T}](./task-comment-item-get.md)
- [{#T}](./task-comment-item-get-list.md)
- [{#T}](./task-comment-item-delete.md)
- [{#T}](../../../tutorials/tasks/how-to-create-comment-with-file.md)