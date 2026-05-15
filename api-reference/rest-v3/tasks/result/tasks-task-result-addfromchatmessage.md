# Add Result from Task Chat Message tasks.task.result.addfromchatmessage

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../../sdk/mcp.md) so that the assistant uses the official REST documentation.

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

The new API call differs by adding the `/api/` parameter in the request:

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

- JS

    The SDK does not currently support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.result.addfromchatmessage',
            {
                fields: {
                    messageId: 335
                }
            }
        );

        const result = response.getData().result;
        console.info(result.item);
    }
    catch( error )
    {
        console.error(error);
    }
    ```

- PHP

    The SDK does not currently support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

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

    The SDK does not currently support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

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

    The SDK does not currently support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

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
