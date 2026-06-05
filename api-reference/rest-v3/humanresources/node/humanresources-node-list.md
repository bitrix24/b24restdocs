# Get a List of Departments humanresources.node.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "View Departments" or "View Teams" access permission.

The method `humanresources.node.list` returns a list of departments or teams.

## Method Parameters

{% include [Note on Parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **type*** 
[`string`](../../../data-types.md) | Type of the structure element.

Possible values:

- `DEPARTMENT` — department
- `TEAM` — team ||
|| **select**
[`array`](../../../data-types.md) | List of fields for the department or team to return.

Available fields:

- `id` — identifier of the structure element
- `name` — name of the department or team
- `type` — type of the structure element
- `structureId` — identifier of the company structure
- `parentId` — identifier of the parent department or team
- `description` — description of the structure element
- `accessCode` — access code of the structure element
- `userCount` — number of users in the department or team
- `colorName` — color of the team
- `xmlId` — external identifier of the structure element
- `createdAt` — creation date and time
- `updatedAt` — last update date and time ||
|| **pagination**
[`object`](../../../data-types.md) | Pagination parameters:
- `page` — page number
- `limit` — number of records per page, default is `50`, maximum is `200`
- `offset` — record offset ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by the addition of the `/api/` parameter in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.node.list`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"DEPARTMENT","select":["id","name","type","structureId","parentId","description","accessCode","userCount","colorName","xmlId","createdAt","updatedAt"],"pagination":{"page":1,"limit":20,"offset":0}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.node.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"DEPARTMENT","select":["id","name","type","structureId","parentId","description","accessCode","userCount","colorName","xmlId","createdAt","updatedAt"],"pagination":{"page":1,"limit":20,"offset":0},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.node.list
    ```

- JS

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'humanresources.node.list',
            {
                type: 'DEPARTMENT',
                select: [
                    'id',
                    'name',
                    'type',
                    'structureId',
                    'parentId',
                    'description',
                    'accessCode',
                    'userCount',
                    'colorName',
                    'xmlId',
                    'createdAt',
                    'updatedAt'
                ],
                pagination: {
                    page: 1,
                    limit: 20,
                    offset: 0
                }
            }
        );

        const result = response.getData().result;
        console.log('Node list:', result);
    }
    catch (error)
    {
        console.error('Error:', error);
    }
    ```

- PHP

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.node.list',
                [
                    'type' => 'DEPARTMENT',
                    'select' => [
                        'id',
                        'name',
                        'type',
                        'structureId',
                        'parentId',
                        'description',
                        'accessCode',
                        'userCount',
                        'colorName',
                        'xmlId',
                        'createdAt',
                        'updatedAt'
                    ],
                    'pagination' => [
                        'page' => 1,
                        'limit' => 20,
                        'offset' => 0
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

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```js
    BX24.callMethod(
        'humanresources.node.list',
        {
            type: 'DEPARTMENT',
            select: [
                'id',
                'name',
                'type',
                'structureId',
                'parentId',
                'description',
                'accessCode',
                'userCount',
                'colorName',
                'xmlId',
                'createdAt',
                'updatedAt'
            ],
            pagination: {
                page: 1,
                limit: 20,
                offset: 0
            }
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    The SDK does not yet support calls to the address /rest/api/. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'humanresources.node.list',
        [
            'type' => 'DEPARTMENT',
            'select' => [
                'id',
                'name',
                'type',
                'structureId',
                'parentId',
                'description',
                'accessCode',
                'userCount',
                'colorName',
                'xmlId',
                'createdAt',
                'updatedAt'
            ],
            'pagination' => [
                'page' => 1,
                'limit' => 20,
                'offset' => 0
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
                    "id": 1,
                    "name": "Sales Department",
                    "type": "DEPARTMENT",
                    "structureId": 1,
                    "parentId": null,
                    "description": "Main sales department",
                    "accessCode": "DR1",
                    "userCount": 18,
                    "colorName": null,
                    "xmlId": null,
                    "createdAt": "2026-05-20T10:15:00+02:00",
                    "updatedAt": "2026-06-02T10:30:00+02:00"
                },
                {
                    "id": 2,
                    "name": "Marketing Department",
                    "type": "DEPARTMENT",
                    "structureId": 1,
                    "parentId": 1,
                    "description": "Department responsible for promoting the company's products",
                    "accessCode": "DR2",
                    "userCount": 9,
                    "colorName": null,
                    "xmlId": "marketing_department",
                    "createdAt": "2026-05-22T09:00:00+02:00",
                    "updatedAt": "2026-06-02T11:45:00+02:00"
                }
            ]
        },
    "time": {
        "start": 1780403500,
        "finish": 1780403500.248911,
        "duration": 0.24891114234924316,
        "processing": 0.21900415420532227,
        "date_start": "2026-06-02T15:31:40+02:00",
        "date_finish": "2026-06-02T15:31:40+02:00",
        "operating_reset_at": 1780404100,
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
[`array`](../../../data-types.md) | Array of department or team objects. The composition of the element fields depends on `select` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION",
        "message": "Cannot recognize pagination parameter `{\"limit\":\"abc\"}`"
    }
}
```

{% include notitle [error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Pagination Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDPAGINATIONEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `limit`
`offset`
`page` | Cannot recognize pagination parameter `#PAGE#` | Provide numeric values. `limit` must not be `0` ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `type` | Required field `type` is not specified | Provide `type` with the value `DEPARTMENT` or `TEAM` ||
|| `type` | An invalid value for the structure element type was provided | Use `DEPARTMENT` for department or `TEAM` for team ||
|#

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` for entity `NodeDto` | Provide only fields from the `select` list ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `select` | Cannot recognize select expression `#SELECT#` | Provide `select` as an array of strings, e.g., `["id","name"]` ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | The user does not have permission to view departments and teams ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Insufficient access rights: required scope is missing | Check the `humanresources` scope ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-node-get.md)
- [{#T}](./humanresources-node-search.md)
- [{#T}](./index.md)