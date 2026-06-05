# Remove Comment from Result tasks.task.result.deleteFromComment

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user with access to the task

The method `tasks.task.result.deleteFromComment` unpins a comment as the result of a task. To remove a comment from the result, use the method [task.commentitem.delete](../comment-item/task-comment-item-delete.md).

A user can only unpin their own comment. An administrator can unpin comments from any user.

{% note warning " " %}

When working with the [new task card](../tasks-new.md) with chat from version `tasks 25.700.0`, the method does not work.

{% endnote %}

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **commentId*** 
[`integer`](../../data-types.md) | The identifier of the comment for which the result needs to be unpinned. 

The comment identifier can be obtained when [adding a new comment](../comment-item/task-comment-item-add.md) or by using the [get list of comments](../comment-item/task-comment-item-get-list.md) method for the task ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // result is null on success (the comment was unfixed from the task result)
    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DeleteFromCommentResult = null

    try {
      const response = await $b24.actions.v2.call.make<DeleteFromCommentResult>({
        method: 'tasks.task.result.deleteFromComment',
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
        console.info('Comment successfully unfixed from task result:', result)
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
      async function deleteFromComment() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'tasks.task.result.deleteFromComment',
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
          console.info('Comment successfully unfixed from task result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteFromComment)
    </script>
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
|| `0` | Access denied. | The user does not have access rights to the task or the comment does not belong to the user ||
|| `100` | Invalid value {value} to match with parameter {commentId}. Should be value of type int. | The value of incorrect type was passed in the `commentId` parameter. It should be of type `integer` ||
|| `0` | Comment not found. | A comment with such an identifier does not exist ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./tasks-task-result-list.md)
- [{#T}](./tasks-task-result-add-from-comment.md)