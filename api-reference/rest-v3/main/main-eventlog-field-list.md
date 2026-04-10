# Get the List of Event Log Fields main.eventlog.field.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`main`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `main.eventlog.field.list` returns a list of available fields for event log entries.

## Method Parameters

{% include [Parameter Note](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | List of description fields to return in the response.

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

{% include [Examples Note](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by the addition of the `/api/` parameter in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/main.eventlog.field.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","title","description","filterable","sortable","multiple"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/main.eventlog.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","title","description","filterable","sortable","multiple"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/main.eventlog.field.list
    ```

- JS

    The SDK does not currently support the address /rest/api/ in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'main.eventlog.field.list',
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
        console.log('Event log fields:', result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    The SDK does not currently support the address /rest/api/ in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'main.eventlog.field.list',
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

    The SDK does not currently support the address /rest/api/ in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```js
    BX24.callMethod(
        'main.eventlog.field.list',
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

    The SDK does not currently support the address /rest/api/ in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'main.eventlog.field.list',
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
                "name": "timestampX",
                "type": "object",
                "title": "timestampX",
                "description": null,
                "filterable": true,
                "sortable": true,
                "multiple": false
            },
            {
                "name": "severity",
                "type": "string",
                "title": "severity",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            },
            {
                "name": "auditTypeId",
                "type": "string",
                "title": "auditTypeId",
                "description": null,
                "filterable": true,
                "sortable": true,
                "multiple": false
            },
            {
                "name": "moduleId",
                "type": "string",
                "title": "moduleId",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            },
            {
                "name": "itemId",
                "type": "string",
                "title": "itemId",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            },
            {
                "name": "remoteAddr",
                "type": "string",
                "title": "remoteAddr",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            },
            {
                "name": "userAgent",
                "type": "string",
                "title": "userAgent",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            },
            {
                "name": "requestUri",
                "type": "string",
                "title": "requestUri",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            },
            {
                "name": "siteId",
                "type": "string",
                "title": "siteId",
                "description": null,
                "filterable": false,
                "sortable": false,
                "multiple": false
            },
            {
                "name": "userId",
                "type": "int",
                "title": "userId",
                "description": null,
                "filterable": true,
                "sortable": true,
                "multiple": false
            },
            {
                "name": "guestId",
                "type": "int",
                "title": "guestId",
                "description": null,
                "filterable": true,
                "sortable": true,
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
            }
        ]
    },
    "time": {
        "start": 1769780464,
        "finish": 1769780464.122003,
        "duration": 0.1220030784606934,
        "processing": 0,
        "date_start": "2026-01-30T16:41:04+02:00",
        "date_finish": "2026-01-30T16:41:04+02:00",
        "operating_reset_at": 1769781064,
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
[`array`](../../data-types.md) | Array of objects describing the fields. The response structure depends on `select`  ||
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
|| `-` | Access denied | No administrator rights or missing main scope ||
|#

#### Errors in the select Parameter

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

- [{#T}](./main-eventlog-field-get.md)
- [{#T}](./main-eventlog-list.md)
- [{#T}](./index.md)