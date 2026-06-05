# Add Comment to Result tasks.task.result.addFromComment

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user with access to the task

The method `tasks.task.result.addFromComment` pins a comment as the result of task execution.

A user can pin only their own comment as a result. An administrator can pin any user's comment, making them the author of the result.

{% note warning " " %}

When working with the [new task detail form](../tasks-new.md) with chat from version `tasks 25.700.0`, the method does not work.

{% endnote %}

## Method Parameters

{% include [Parameter Note](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **commentId***
[`integer`](../../data-types.md) | The identifier of the comment to be pinned as a result.

The comment identifier can be obtained when [adding a new comment](../comment-item/task-comment-item-add.md) or by using the [method to get the list of comments](../comment-item/task-comment-item-get-list.md) ||
|#

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type TaskResultAddResult = {
      id: number
      taskId: number
      commentId: number
      createdBy: number
      createdAt: ISODate
      updatedAt: ISODate
      status: number
      text: string
      formattedText: string
      files: null
    }

    try {
      const response = await $b24.actions.v2.call.make<TaskResultAddResult>({
        method: 'tasks.task.result.addFromComment',
        params: {
          commentId: 3199,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.id, result.taskId, result.text)
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
      async function addTaskResultFromComment() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.task.result.addFromComment',
            params: {
              commentId: 3199,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.id, result.taskId, result.text)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addTaskResultFromComment)
    </script>
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

HTTP Status: **200**

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
[`object`](../../data-types.md) | An object describing the pinned result ||
|| **id**
[`integer`](../../data-types.md) | The identifier of the result ||
|| **taskId**
[`integer`](../../data-types.md) | The identifier of the task ||
|| **commentId**
[`integer`](../../data-types.md) | The identifier of the comment pinned as a result ||
|| **createdBy**
[`integer`](../../data-types.md) | The identifier of the user who pinned the result ||
|| **createdAt**
[`string`](../../data-types.md) | The date and time the result was pinned in ISO 8601 format ||
|| **updatedAt**
[`string`](../../data-types.md) | The date and time of the last modification of the result in ISO 8601 format ||
|| **status**
[`integer`](../../data-types.md) | The status of the result. Possible values:
- `0` — result is open
- `1` — result is closed

The result becomes closed after the task is completed and retains this status after the task is resumed. Only new results in an unfinished task will be open.

A comment with an open result cannot be added again to the result. If the result is closed, adding is possible
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

HTTP Status: **400**

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
|| `0` | Result already exists. | The comment is already pinned as a result ||
|| `100` | Invalid value {value} to match with parameter {commentId}. Should be value of type int. | The parameter `commentId` has an invalid type value. It should be of type `integer` ||
|| `0` | Comment not found. | A comment with such an identifier does not exist ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-task-result-list.md)
- [{#T}](./tasks-task-result-delete-from-comment.md)