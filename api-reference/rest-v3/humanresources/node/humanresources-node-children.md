# Get Child Departments humanresources.node.children

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "View Departments" or "View Teams" access permission

The method `humanresources.node.children` returns the direct child departments and teams of the selected structure element.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **id*** 
[`integer`](../../../data-types.md) | Identifier of the parent department or team.

The identifier can be obtained using the [humanresources.node.list](./humanresources-node-list.md) method. ||
|| **select** 
[`array`](../../../data-types.md) | List of fields of the department or team to return.

Available fields:

- `id` — identifier of the structure element
- `name` — name of the department or team
- `type` — type of the structure element
- `structureId` — identifier of the company structure
- `parentId` — identifier of the parent department or team
- `description` — description of the structure element
- `accessCode` — access code of the structure element
- `userCount` — number of users in the department or team
- `colorName` — team color
- `xmlId` — external identifier of the structure element
- `createdAt` — creation date and time
- `updatedAt` — last update date and time ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.node.children`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"select":["id","name","type","structureId","parentId","description","accessCode","userCount","colorName","xmlId","createdAt","updatedAt"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.node.children
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"select":["id","name","type","structureId","parentId","description","accessCode","userCount","colorName","xmlId","createdAt","updatedAt"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.node.children
    ```

- JS

    The SDK does not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'humanresources.node.children',
            {
                id: 1,
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
                ]
            }
        );

        const result = response.getData().result.items;
        console.info(result.length);
    }
    catch (error)
    {
        console.error(error);
    }
    ```

- PHP

    The SDK does not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.node.children',
                [
                    'id' => 1,
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
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting children: ' . $e->getMessage();
    }
    ```

- BX24.js

    The SDK does not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```js
    BX24.callMethod(
        'humanresources.node.children',
        {
            id: 1,
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
            ]
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    The SDK does not yet support the /rest/api/ address in calls. Use direct HTTP requests, for example, via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'humanresources.node.children',
        [
            'id' => 1,
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
            ]
        }
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
                "id": 12,
                "name": "B2B Sales",
                "type": "DEPARTMENT",
                "structureId": 1,
                "parentId": 1,
                "description": "Working with corporate clients",
                "accessCode": "DR12",
                "userCount": 9,
                "colorName": null,
                "xmlId": null,
                "createdAt": "2026-05-20T10:15:00+02:00",
                "updatedAt": "2026-06-01T16:30:00+02:00"
            },
            {
                "id": 18,
                "name": "Marketing",
                "type": "DEPARTMENT",
                "structureId": 1,
                "parentId": 1,
                "description": "Promotion of products and brand",
                "accessCode": "DR18",
                "userCount": 6,
                "colorName": null,
                "xmlId": null,
                "createdAt": "2026-05-22T11:40:00+02:00",
                "updatedAt": "2026-06-03T10:05:00+02:00"
            }
        ]
    },
    "time": {
        "start": 1780396400,
        "finish": 1780396400.428761,
        "duration": 0.42876100540161133,
        "processing": 0.3972141742706299,
        "date_start": "2026-06-02T13:33:20+02:00",
        "date_finish": "2026-06-02T13:33:20+02:00",
        "operating_reset_at": 1780397000,
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
[`array`](../../../data-types.md) | Array of child departments and teams. The composition of the element fields depends on `select` ||
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
                "message": "Required field `id` is not specified",
                "field": "id"
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
|| `id` | Required field `id` is not specified | Add `id` to the request body ||
|| `id` | Field `id` requires data type `int` for this request | Ensure that the value of `id` is passed as a number ||
|| `select` | Unsupported field name provided for selection | Use only values from the description of the `select` parameter ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied | The user does not have permission to view the specified department or team ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Insufficient access rights: required scope is missing | Check the `humanresources` scope ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-node-get.md)
- [{#T}](./humanresources-node-add.md)
- [{#T}](./humanresources-node-list.md)
- [{#T}](./index.md)