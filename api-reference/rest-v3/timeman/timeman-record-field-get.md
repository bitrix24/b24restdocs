# Get the Record Field of Working Time timeman.record.field.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`timeman`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `timeman.record.field.get` returns the description of the working time record field by name.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **name*** 
[`string`](../../data-types.md) | The name of the field whose description is to be retrieved.

Available fields:

- `id` — record identifier
- `userId` — employee identifier
- `startTime` — start time of the working interval
- `endTime` — end time of the working interval
- `duration` — duration of the working interval in seconds
- `breakLength` — duration of breaks in seconds
- `state` — status of the record
- `isApproved` — approval status of the record ||
|| **select**
[`array`](../../data-types.md) | List of description fields to return in the response.

Available fields:

- `name` — field name
- `type` — data type
- `title` — title
- `description` — description
- `validationRules` — validation rules
- `requiredGroups` — required groups
- `filterable` — filter availability status
- `sortable` — sort availability status
- `editable` — editability status
- `multiple` — multiple value status
- `elementType` — element type for composite fields ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by the addition of the `/api/` parameter in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/timeman.record.field.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"startTime","select":["name","type","title","filterable","sortable"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/timeman.record.field.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"startTime","select":["name","type","title","filterable","sortable"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/timeman.record.field.get
    ```

- JS

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, such as via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'timeman.record.field.get',
            {
                name: 'startTime',
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
        console.log('Record field:', result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, such as via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'timeman.record.field.get',
                [
                    'name' => 'startTime',
                    'select' => [
                        'name',
                        'type',
                        'title',
                        'filterable',
                        'sortable'
                    ],
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

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, such as via curl or fetch.

    ```js
    BX24.callMethod(
        'timeman.record.field.get',
        {
            name: 'startTime',
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

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, such as via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'timeman.record.field.get',
        [
            'name' => 'startTime',
            'select' => [
                'name',
                'type',
                'title',
                'filterable',
                'sortable'
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
            "name": "startTime",
            "type": "object",
            "title": "startTime",
            "filterable": true,
            "sortable": true
        }
    },
    "time": {
        "start": 1780410633,
        "finish": 1780410633.157809,
        "duration": 0.15780901908874512,
        "processing": 0,
        "date_start": "2026-06-02T17:30:33+02:00",
        "date_finish": "2026-06-02T17:30:33+02:00",
        "operating_reset_at": 1780411233,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with response data ||
|| **item**
[`object`](../../data-types.md) | Object with field description. The response structure depends on `select` ||
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
|| `-` | Access denied | Check read permissions for working time and scope `timeman` ||
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

#### Select Parameter Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` for object `DtoFieldDto` | Pass only fields from the list: `name`, `type`, `title`, `description`, `validationRules`, `requiredGroups`, `filterable`, `sortable`, `editable`, `multiple`, `elementType` ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unable to recognize select expression `#SELECT#` | Pass `select` as an array of strings, e.g., `["name","type"]` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./timeman-record-field-list.md)
- [{#T}](./timeman-record-list.md)
- [{#T}](./index.md)