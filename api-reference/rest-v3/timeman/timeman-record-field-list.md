# Get a List of Time Tracking Record Fields timeman.record.field.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.record.field.list` returns a list of available fields for time tracking records.

## Method Parameters

{% include [Note on Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | List of field descriptions to return in the response.

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

{% include [Note on Examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/timeman.record.field.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","title","filterable","sortable"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/timeman.record.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","title","filterable","sortable"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/timeman.record.field.list
    ```

- JS

    The SDK does not currently support the /rest/api/ address in calls. Use direct HTTP requests, such as via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'timeman.record.field.list',
            {
                select: [
                    'name',
                    'type',
                    'title',
                    'filterable',
                    'sortable'
                ]
            }
        );

        const result = response.getData().result;
        console.log('Record fields:', result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    The SDK does not currently support the /rest/api/ address in calls. Use direct HTTP requests, such as via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'timeman.record.field.list',
                [
                    'select' => [
                        'name',
                        'type',
                        'title',
                        'filterable',
                        'sortable'
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

    The SDK does not currently support the /rest/api/ address in calls. Use direct HTTP requests, such as via curl or fetch.

    ```js
    BX24.callMethod(
        'timeman.record.field.list',
        {
            select: [
                'name',
                'type',
                'title',
                'filterable',
                'sortable'
            ]
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    The SDK does not currently support the /rest/api/ address in calls. Use direct HTTP requests, such as via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'timeman.record.field.list',
        [
            'select' => [
                'name',
                'type',
                'title',
                'filterable',
                'sortable'
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
                "filterable": false,
                "sortable": true
            },
            {
                "name": "userId",
                "type": "int",
                "title": "userId",
                "filterable": true,
                "sortable": true
            },
            {
                "name": "startTime",
                "type": "object",
                "title": "startTime",
                "filterable": true,
                "sortable": true
            },
            {
                "name": "endTime",
                "type": "object",
                "title": "endTime",
                "filterable": false,
                "sortable": true
            },
            {
                "name": "duration",
                "type": "int",
                "title": "duration",
                "filterable": false,
                "sortable": true
            },
            {
                "name": "breakLength",
                "type": "int",
                "title": "breakLength",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "state",
                "type": "object",
                "title": "state",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "isApproved",
                "type": "bool",
                "title": "isApproved",
                "filterable": false,
                "sortable": false
            }
        ]
    },
    "time": {
        "start": 1780410881,
        "finish": 1780410881.429648,
        "duration": 0.42964792251586914,
        "processing": 0,
        "date_start": "2026-06-02T17:34:41+02:00",
        "date_finish": "2026-06-02T17:34:41+02:00",
        "operating_reset_at": 1780411481,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object containing response data ||
|| **items**
[`array`](../../data-types.md) | Array of objects describing fields. The response structure depends on `select` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION",
        "message": "Unknown field `unknownField` for object `DtoFieldDto`"
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | Check time tracking read permissions and the `timeman` scope ||
|#

#### Errors in the select parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` for object `DtoFieldDto` | Only pass fields from the list: `name`, `type`, `title`, `description`, `validationRules`, `requiredGroups`, `filterable`, `sortable`, `editable`, `multiple`, `elementType` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unable to recognize select expression `#SELECT#` | Pass `select` as an array of strings, e.g., `["name","type"]` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./timeman-record-field-get.md)
- [{#T}](./timeman-record-list.md)
- [{#T}](./index.md)