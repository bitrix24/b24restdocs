# Add Result from Task Chat Message tasks.task.result.addfromchatmessage

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../../ai-tools/mcp.md) so that the assistant uses the official REST documentation.

{% endnote %}

> Scope: [`tasks`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to the task

The method `tasks.task.result.addfromchatmessage` creates a task result from a task chat message.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with [message fields](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **messageId***
[`integer`](../../../data-types.md) | Identifier of the task chat message.

The identifier can be obtained when sending a message using the [tasks.task.chat.message.send](../tasks-task-chat-message-send.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

```text
https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.task.result.addfromchatmessage
```

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"messageId":335}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.result.addfromchatmessage
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"messageId":335},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.task.result.addfromchatmessage
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AddFromChatMessageResult = {
      item: {
        id: number
        taskId: number
        text: string
        authorId: number
        createdAt: ISODate
        updatedAt: ISODate | null
        status: string
        fileIds: number[]
        rights: {
          edit: boolean
          remove: boolean
        }
        messageId: number
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<AddFromChatMessageResult>({
        method: 'tasks.task.result.addfromchatmessage',
        params: {
          fields: {
            messageId: 335,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.item.id, result.item.taskId, result.item.text)
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
      async function addResultFromChatMessage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'tasks.task.result.addfromchatmessage',
            params: {
              fields: {
                messageId: 335,
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
          console.info(result.item.id, result.item.taskId, result.item.text)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addResultFromChatMessage)
    </script>
    ```

- PHP

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.result.addfromchatmessage',
                [
                    'fields' => [
                        'messageId' => 335,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```js
    BX24.callMethod(
        'tasks.task.result.addfromchatmessage',
        {
            fields: {
                messageId: 335
            }
        },
        function(result){
            console.info(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.result.addfromchatmessage',
        [
            'fields' => [
                'messageId' => 335,
            ],
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
        "item": {
            "id": 18,
            "taskId": 51,
            "text": "Task work completed",
            "authorId": 1,
            "createdAt": "2026-04-30T10:25:00+02:00",
            "updatedAt": null,
            "status": "open",
            "fileIds": [],
            "rights": {
                "edit": true,
                "remove": true
            },
            "messageId": 335
        }
    },
    "time": {
        "start": 1777530300,
        "finish": 1777530300.123456,
        "duration": 0.123456,
        "processing": 0.1,
        "date_start": "2026-04-30T10:25:00+02:00",
        "date_finish": "2026-04-30T10:25:00+02:00",
        "operating_reset_at": 1777530900,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data ||
|| **item**
[`object`](../../../data-types.md) | Object of the task result.

The fields of the object match the response of the method [tasks.task.result.add](./tasks-task-result-add.md#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error during request object validation",
        "validation": [
            {
                "field": "fields",
                "message": "Required field `fields` is missing"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `messageId` | Required field `messageId` is missing | Provide the message identifier in `fields.messageId` ||
|| `fileIds` | Field `fileIds` is not available for filling | Do not pass `fileIds` in the `fields` parameter ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Provide a `fields` object with the `messageId` field ||
|| `messageId` | Field `messageId` requires data type `int` for this request | Provide `messageId` as a number ||
|| `messageId` | `Message is not from task chat` | Specify the message identifier from the task chat ||
|| `taskId` | `Task not found` | Check that the task associated with the chat exists ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `fields` | Unknown field for entity `ResultDto` | Only pass the `messageId` field in `fields` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP Status: **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `messageId` | Access denied | Check user permissions to create a result from the chat message ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-task-result-add.md)
- [{#T}](./tasks-task-result-update.md)
- [{#T}](./tasks-task-result-list.md)
- [{#T}](./tasks-task-result-delete.md)
