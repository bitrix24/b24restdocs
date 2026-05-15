# Delete Task Result tasks.task.result.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../../sdk/mcp.md) so that the assistant can use the official REST documentation.

{% endnote %}

> Scope: [`tasks`](../../../scopes/permissions.md)
>
> Who can execute the method: author of the result

The method `tasks.task.result.delete` deletes the task result.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the result ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.task.result.delete`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":17}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.result.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":17,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.task.result.delete
    ```

- JS

    The SDK does not currently support calls to the address /rest/api/. Use direct HTTP requests, such as through curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.result.delete',
            {
                id: 17
            }
        );

        const result = response.getData().result;
        console.info(result);
    }
    catch( error )
    {
        console.error(error);
    }
    ```

- PHP

    The SDK does not currently support calls to the address /rest/api/. Use direct HTTP requests, such as through curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.result.delete',
                [
                    'id' => 17,
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

    The SDK does not currently support calls to the address /rest/api/. Use direct HTTP requests, such as through curl or fetch.

    ```js
    BX24.callMethod(
        'tasks.task.result.delete',
        {
            id: 17
        },
        function(result){
            console.info(result.data());
        }
    );
    ```

- PHP CRest

    The SDK does not currently support calls to the address /rest/api/. Use direct HTTP requests, such as through curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.result.delete',
        [
            'id' => 17,
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
        "result": true
    },
    "time": {
        "start": 1777530600,
        "finish": 1777530600.123456,
        "duration": 0.123456,
        "processing": 0.1,
        "date_start": "2026-04-30T10:30:00+02:00",
        "date_finish": "2026-04-30T10:30:00+02:00",
        "operating_reset_at": 1777531200,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with the deletion result [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Returns `true` if the result was deleted ||
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
                "message": "At least one required field must be filled: id, filter"
            },
            {
                "message": "Only one of the values must be provided: id, filter"
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
`filter` | At least one required field must be filled: `id`, `filter` | Provide the result identifier in the `id` parameter ||
|| `id` | Required field `id` is missing | Provide the result identifier in the `id` parameter ||
|| `id`
`filter` | Only one of the values must be provided: `id`, `filter` | Provide only the `id` parameter ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP Status: **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `id` | Access denied | Check the user's permissions to delete the result ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-task-result-add.md)
- [{#T}](./tasks-task-result-addfromchatmessage.md)
- [{#T}](./tasks-task-result-update.md)
- [{#T}](./tasks-task-result-list.md)