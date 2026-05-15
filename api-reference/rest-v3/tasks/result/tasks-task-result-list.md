# Get the list of task results tasks.task.result.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../../sdk/mcp.md) so that the assistant can use the official REST documentation.

{% endnote %}

> Scope: [`tasks`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to the task

The method `tasks.task.result.list` returns a list of task results.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **order**
[`object`](../../../data-types.md) | An object for sorting the list of results in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

The sorting direction can take the following values:

- `ASC` — ascending
- `DESC` — descending

The field for sorting can take the following values:
- `id` — result identifier
- `authorId` — author identifier of the result
- `createdAt` — creation date of the result
- `updatedAt` — update date of the result
- `status` — status of the result
- `messageId` — chat message identifier ||
|| **filter***
[`array`](../../../data-types.md) | An array of conditions for filtering the list of results. Be sure to pass the condition for `taskId`.

Each condition is an array with the field name, operator, and value. For the operator `=`, you can only pass the field name and value.

Condition formats:
- `["taskId", 51]`
- `["id", ">=", 17]`
- `["id", "in", [17, 18, 19]]`

Available filter operators: `=`, `>`, `<`, `<=`, `>=`, `!=`, `in`, `between`.

- `=` — equals
- `>` — greater than
- `<` — less than
- `<=` — less than or equal to
- `>=` — greater than or equal to
- `!=` — not equal
- `in` — is in the list of values
- `between` — is within the range of values

The filterable field can take the following values:
- `taskId` — task identifier. Required field
- `id` — result identifier
- `authorId` — author identifier of the result
- `status` — status of the result. Possible values: `open`, `closed`
- `messageId` — chat message identifier from which the result was created ||
|| **select**
[`array`](../../../data-types.md) | An array of fields to select.

If the parameter is not passed, the method will return the default fields.

The field can take the following values:
- `id` — result identifier
- `taskId` — task identifier
- `text` — result text
- `authorId` — author identifier of the result
- `author` — author data
- `createdAt` — creation date of the result
- `updatedAt` — update date of the result
- `status` — status of the result
- `fileIds` — identifiers of result files
- `rights` — current user's rights on the result
- `messageId` — chat message identifier ||
|| **pagination**
[`object`](../../../data-types.md) | An object for managing pagination.

Pagination parameters:
- `page` — page number
- `limit` — number of items per page
- `offset` — offset from the beginning of the list ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the parameter `/api/` in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.task.result.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["taskId",51]],"select":["id","taskId","text","authorId","createdAt","status","messageId"],"order":{"createdAt":"DESC"},"pagination":{"limit":10,"offset":0}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.result.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"filter":[["taskId",51]],"select":["id","taskId","text","authorId","createdAt","status","messageId"],"order":{"createdAt":"DESC"},"pagination":{"limit":10,"offset":0},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.task.result.list
    ```

- JS

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.result.list',
            {
                filter: [
                    ['taskId', 51]
                ],
                select: ['id', 'taskId', 'text', 'authorId', 'createdAt', 'status', 'messageId'],
                order: {
                    createdAt: 'DESC'
                },
                pagination: {
                    limit: 10,
                    offset: 0
                }
            }
        );

        const result = response.getData().result;
        console.info(result.items);
    }
    catch( error )
    {
        console.error(error);
    }
    ```

- PHP

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.result.list',
                [
                    'filter' => [
                        ['taskId', 51],
                    ],
                    'select' => ['id', 'taskId', 'text', 'authorId', 'createdAt', 'status', 'messageId'],
                    'order' => [
                        'createdAt' => 'DESC',
                    ],
                    'pagination' => [
                        'limit' => 10,
                        'offset' => 0,
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

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```js
    BX24.callMethod(
        'tasks.task.result.list',
        {
            filter: [
                ['taskId', 51]
            ],
            select: ['id', 'taskId', 'text', 'authorId', 'createdAt', 'status', 'messageId'],
            order: {
                createdAt: 'DESC'
            },
            pagination: {
                limit: 10,
                offset: 0
            }
        },
        function(result){
            console.info(result.data());
        }
    );
    ```

- PHP CRest

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.result.list',
        [
            'filter' => [
                ['taskId', 51],
            ],
            'select' => ['id', 'taskId', 'text', 'authorId', 'createdAt', 'status', 'messageId'],
            'order' => [
                'createdAt' => 'DESC',
            ],
            'pagination' => [
                'limit' => 10,
                'offset' => 0,
            ],
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
    "result": {
        "items": [
            {
                "id": 17,
                "taskId": 51,
                "text": "Task work completed",
                "authorId": 1,
                "createdAt": "2026-04-30T10:15:00+02:00",
                "status": "open",
                "messageId": null
            }
        ]
    },
    "time": {
        "start": 1777529700,
        "finish": 1777529700.123456,
        "duration": 0.123456,
        "processing": 0.1,
        "date_start": "2026-04-30T10:15:00+02:00",
        "date_finish": "2026-04-30T10:15:00+02:00",
        "operating_reset_at": 1777530300,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | An object with response data [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **items**
[`array`](../../../data-types.md) | An array of task results.

The fields of the elements depend on the `select` parameter and match the fields of the result object in the method [tasks.task.result.add](./tasks-task-result-add.md#item) ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating the request object",
        "validation": [
            {
                "field": "filter",
                "message": "The field value cannot be empty"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter` | The field value cannot be empty | Pass the filter as an array of conditions ||
|| `taskId` | The `taskId` field requires an `int` data type for this request | Pass `taskId` as a number ||
|#

Error code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTFILTERVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter.taskId` | A filter for the required field `taskId` must be specified | Pass a condition for `taskId` in the `filter` parameter ||
|#

Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter` | Unable to recognize the filter expression | Pass the filter as an array of conditions ||
|#

Error code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter` | Unknown filter operator | Use operators `=`, `>`, `<`, `<=`, `>=`, `!=`, `in`, `between` ||
|#

Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `pagination` | Unable to recognize the pagination parameter | Pass `page`, `limit`, or `offset` as numbers ||
|#

Error code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `filter`
`select`
`order` | Unknown field `#FIELD#` for entity `ResultDto` | Pass only fields from the list: `id`, `taskId`, `text`, `authorId`, `author`, `createdAt`, `updatedAt`, `status`, `fileIds`, `rights`, `messageId` ||
|#

Error code: `BITRIX_REST_V3_EXCEPTION_INVALIDORDEREXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `order` | Unable to recognize the sorting expression | Pass the sorting direction `ASC` or `DESC` ||
|#

#### Access Error

Error code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `taskId` | Access denied | Check the user's rights to read the task ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-task-result-add.md)
- [{#T}](./tasks-task-result-addfromchatmessage.md)
- [{#T}](./tasks-task-result-update.md)
- [{#T}](./tasks-task-result-delete.md)