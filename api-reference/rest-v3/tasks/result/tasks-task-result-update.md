# Update Task Result tasks.task.result.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can use the official REST documentation.

{% endnote %}

> Scope: [`tasks`](../../../scopes/permissions.md)
>
> Who can execute the method: author of the result

The method `tasks.task.result.update` updates the text of the task result.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the result ||
|| **fields***
[`object`](../../../data-types.md) | Object with [result fields](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **text***
[`string`](../../../data-types.md) | New text of the result ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.task.result.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":17,"fields":{"text":"Task has been completed and verified"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.result.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":17,"fields":{"text":"Task has been completed and verified"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.task.result.update
    ```

- JS

    The SDK does not currently support calls to the /rest/api/ address. Use direct HTTP requests, for example, via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.result.update',
            {
                id: 17,
                fields: {
                    text: 'Task has been completed and verified'
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

    The SDK does not currently support calls to the /rest/api/ address. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.result.update',
                [
                    'id' => 17,
                    'fields' => [
                        'text' => 'Task has been completed and verified',
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

    The SDK does not currently support calls to the /rest/api/ address. Use direct HTTP requests, for example, via curl or fetch.

    ```js
    BX24.callMethod(
        'tasks.task.result.update',
        {
            id: 17,
            fields: {
                text: 'Task has been completed and verified'
            }
        },
        function(result){
            console.info(result.data());
        }
    );
    ```

- PHP CRest

    The SDK does not currently support calls to the /rest/api/ address. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.result.update',
        [
            'id' => 17,
            'fields' => [
                'text' => 'Task has been completed and verified',
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
            "id": 17,
            "taskId": 51,
            "text": "Task has been completed and verified",
            "authorId": 1,
            "createdAt": "2026-04-30T10:15:00+02:00",
            "updatedAt": "2026-04-30T10:20:00+02:00",
            "status": "open",
            "fileIds": [],
            "rights": {
                "edit": true,
                "remove": true
            },
            "messageId": null
        }
    },
    "time": {
        "start": 1777530000,
        "finish": 1777530000.123456,
        "duration": 0.123456,
        "processing": 0.1,
        "date_start": "2026-04-30T10:20:00+02:00",
        "date_finish": "2026-04-30T10:20:00+02:00",
        "operating_reset_at": 1777530600,
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
        "message": "Error validating the request object",
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

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `id`
`filter` | At least one required field is not filled: `id`, `filter` | Provide the result identifier in the `id` parameter ||
|| `id`
`filter` | Only one of the values must be provided: `id`, `filter` | Provide only one parameter: `id` or `filter` ||
|| `fields` | Required field `fields` is missing | Provide a `fields` object with the `text` field ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `text` | Required field `text` is missing | Provide the new result text in `fields.text` ||
|| `fileIds` | Field `fileIds` is not available for filling | Do not provide `fileIds` in the `fields` parameter ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `fields` | Unknown field for entity `ResultDto` | Only provide the `text` field in `fields` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP Status: **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `id` | Access denied | Check the user's permissions to modify the result ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-task-result-add.md)
- [{#T}](./tasks-task-result-addfromchatmessage.md)
- [{#T}](./tasks-task-result-list.md)
- [{#T}](./tasks-task-result-delete.md)