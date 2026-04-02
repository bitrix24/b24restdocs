# Get Chat Message Field of Task tasks.task.chat.message.field.get

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.chat.message.field.get` returns the description of a chat message field for a task by name.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | The name of the field whose description is to be retrieved ||
|| **select**
[`array`](../../data-types.md) | A list of description fields to return in the response.

Available fields:
- `name` — field name
- `type` — data type
- `title` — title
- `description` — description
- `validationRules` — validation rules
- `requiredGroups` — required groups
- `filterable` — filter availability indicator
- `sortable` — sort availability indicator
- `editable` — editability indicator
- `multiple` — multiple value indicator
- `elementType` — element type for composite fields ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% note info "" %}

The call to the new API differs by the addition of the `/api/` parameter in the request:

`https://{address}/rest/api/{user_id}/{webhook_token}/tasks.task.chat.message.field.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"taskId","select":["name","type","title","description","filterable","sortable","multiple"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.chat.message.field.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"taskId","select":["name","type","title","description","filterable","sortable","multiple"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.task.chat.message.field.get
    ```

- JS

    The SDK does not currently support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.chat.message.field.get',
            {
                name: 'taskId',
                select: [
                    'name',
                    'type',
                    'title',
                    'description',
                    'filterable',
                    'sortable',
                    'multiple'
                ]
            }
        );

        const result = response.getData().result;
        console.log('Field item:', result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    The SDK does not currently support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.chat.message.field.get',
                [
                    'name' => 'taskId',
                    'select' => [
                        'name',
                        'type',
                        'title',
                        'description',
                        'filterable',
                        'sortable',
                        'multiple'
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    The SDK does not currently support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```js
    BX24.callMethod(
        'tasks.task.chat.message.field.get',
        {
            name: 'taskId',
            select: [
                'name',
                'type',
                'title',
                'description',
                'filterable',
                'sortable',
                'multiple'
            ]
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    The SDK does not currently support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.chat.message.field.get',
        [
            'name' => 'taskId',
            'select' => [
                'name',
                'type',
                'title',
                'description',
                'filterable',
                'sortable',
                'multiple'
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
    "result": {
        "item": {
            "name": "taskId",
            "type": "int",
            "title": "taskId",
            "description": null,
            "filterable": false,
            "sortable": false,
            "multiple": false
        }
    },
    "time": {
        "start": 1773649754,
        "finish": 1773649754.213566,
        "duration": 0.21356606483459473,
        "processing": 0,
        "date_start": "2026-03-16T11:29:14+01:00",
        "date_finish": "2026-03-16T11:29:14+01:00",
        "operating_reset_at": 1773650354,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object containing the response data ||
|| **item**
[`object`](../../data-types.md) | Object describing the field. The response structure depends on `select`  ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
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
                "field": "name",
                "message": "Required field `name` is not specified"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Check user permissions and scope `task` ||
|#

#### Data Not Found Errors

Error Code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `name` | Field `#FIELD#` not found | Specify an existing field name ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `name` | Required field `name` is not specified | Pass the `name` parameter with an existing field name ||
|#

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` for entity `DtoFieldDto` | Pass only fields from the list: `name`, `type`, `title`, `description`, `validationRules`, `requiredGroups`, `filterable`, `sortable`, `editable`, `multiple`, `elementType` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unable to recognize select expression `#SELECT#` | Pass `select` as an array of strings, for example `["name","type"]` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-task-chat-message-field-list.md)
- [{#T}](./tasks-task-chat-message-send.md)
- [{#T}](./index.md)