# Get the List of Task Fields tasks.task.field.list

> Scope: [`tasks`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.field.list` returns a list of available task fields.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | List of field descriptions to be returned in the response.

Available fields:
- `name` — field name
- `type` — data type
- `title` — title
- `description` — description
- `validationRules` — validation rules
- `requiredGroups` — mandatory groups
- `filterable` — filter availability indicator
- `sortable` — sort availability indicator
- `editable` — editability indicator
- `multiple` — multiple value indicator
- `elementType` — element type for composite fields ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by the addition of the `/api/` parameter in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/tasks.task.field.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","title","description","filterable","sortable","multiple"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","title","description","filterable","sortable","multiple"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/tasks.task.field.list
    ```

- JS

    The SDK does not currently support calls to the address /rest/api/. Use direct HTTP requests, such as via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'tasks.task.field.list',
            {
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
        console.log('Field list:', result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    The SDK does not currently support calls to the address /rest/api/. Use direct HTTP requests, such as via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'tasks.task.field.list',
                [
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

    The SDK does not currently support calls to the address /rest/api/. Use direct HTTP requests, such as via curl or fetch.

    ```js
    BX24.callMethod(
        'tasks.task.field.list',
        {
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

    The SDK does not currently support calls to the address /rest/api/. Use direct HTTP requests, such as via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'tasks.task.field.list',
        [
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
        "items": [
            {
                "name": "id",
                "type": "int",
                "title": "id",
                "description": null,
                "filterable": true,
                "sortable": true,
                "multiple": false
            },
            {
                "name": "title",
                "type": "string",
                "title": "title",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            },
            {
                "name": "description",
                "type": "string",
                "title": "description",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            },
            {
                "name": "creatorId",
                "type": "int",
                "title": "creatorId",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            },
            {
                "name": "creator",
                "type": "object",
                "title": "creator",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            },
            {
                "name": "created",
                "type": "object",
                "title": "created",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            },
            {
                "name": "responsibleId",
                "type": "int",
                "title": "responsibleId",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            },
            {
                "name": "responsible",
                "type": "object",
                "title": "responsible",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            },
            // ...
            {
                "name": "inMute",
                "type": "array",
                "title": "inMute",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            },
            {
                "name": "source",
                "type": "object",
                "title": "source",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            },
            {
                "name": "dependsOn",
                "type": "array",
                "title": "dependsOn",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            },
            {
                "name": "scenarios",
                "type": "array",
                "title": "scenarios",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            }
        ]
    },
    "time": {
        "start": 1773648753,
        "finish": 1773648753.478823,
        "duration": 0.4788229465484619,
        "processing": 0,
        "date_start": "2026-03-16T11:12:33+01:00",
        "date_finish": "2026-03-16T11:12:33+01:00",
        "operating_reset_at": 1773649353,
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
|| **items**
[`array`](../../data-types.md) | Array of objects describing the fields. The response structure depends on `select` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION",
        "message": "Unknown field `unknownField` for entity `DtoFieldDto`"
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

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` for entity `DtoFieldDto` | Only pass fields from the list: `name`, `type`, `title`, `description`, `validationRules`, `requiredGroups`, `filterable`, `sortable`, `editable`, `multiple`, `elementType` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unable to recognize select expression `#SELECT#` | Pass `select` as an array of strings, for example `["name","type"]` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-task-field-get.md)
- [{#T}](./tasks-task-get.md)
- [{#T}](./index.md)