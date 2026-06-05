# Get a List of Fields from the humanresources.node.field.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `humanresources.node.field.list` returns a list of available fields in the department.

## Method Parameters

{% include [Note on Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | List of description fields to return in the response.

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

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.node.field.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","title","filterable","sortable"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.node.field.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["name","type","title","filterable","sortable"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.node.field.list
    ```

- JS

    The SDK does not yet support calls to the /rest/api/ address. Use direct HTTP requests, such as through curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'humanresources.node.field.list',
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
        console.log('Field list:', result);
    }
    catch (error)
    {
        console.error('Error:', error);
    }
    ```

- PHP

    The SDK does not yet support calls to the /rest/api/ address. Use direct HTTP requests, such as through curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.node.field.list',
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

    The SDK does not yet support calls to the /rest/api/ address. Use direct HTTP requests, such as through curl or fetch.

    ```js
    BX24.callMethod(
        'humanresources.node.field.list',
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

    The SDK does not yet support calls to the /rest/api/ address. Use direct HTTP requests, such as through curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'humanresources.node.field.list',
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
                "filterable": true,
                "sortable": true
            },
            {
                "name": "name",
                "type": "string",
                "title": "name",
                "filterable": false,
                "sortable": true
            },
            {
                "name": "type",
                "type": "string",
                "title": "type",
                "filterable": true,
                "sortable": false
            },
            {
                "name": "structureId",
                "type": "int",
                "title": "structureId",
                "filterable": true,
                "sortable": false
            },
            {
                "name": "parentId",
                "type": "int",
                "title": "parentId",
                "filterable": true,
                "sortable": false
            },
            {
                "name": "description",
                "type": "string",
                "title": "description",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "accessCode",
                "type": "string",
                "title": "accessCode",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "userCount",
                "type": "int",
                "title": "userCount",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "colorName",
                "type": "string",
                "title": "colorName",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "xmlId",
                "type": "string",
                "title": "xmlId",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "createdAt",
                "type": "string",
                "title": "createdAt",
                "filterable": false,
                "sortable": true
            },
            {
                "name": "updatedAt",
                "type": "string",
                "title": "updatedAt",
                "filterable": false,
                "sortable": false
            },
            {
                "name": "members",
                "type": "array",
                "title": "members",
                "filterable": false,
                "sortable": false
            }
        ]
    },
    "time": {
        "start": 1780402300,
        "finish": 1780402300.162847,
        "duration": 0.16284704208374023,
        "processing": 0,
        "date_start": "2026-06-02T15:11:40+02:00",
        "date_finish": "2026-06-02T15:11:40+02:00",
        "operating_reset_at": 1780402900,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object containing the response data ||
|| **items**
[`array`](../../../data-types.md) | Array of objects describing the fields. The response structure depends on `select` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
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

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION`

#|
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Insufficient access rights: required scope is missing | Check the `humanresources` scope ||
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
|| `select` | Unable to recognize select expression `#SELECT#` | Pass `select` as an array of strings, e.g., `["name","type"]` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-node-field-get.md)
- [{#T}](./index.md)