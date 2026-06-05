# Update Department humanresources.node.edit

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`humanresources`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit Departments" or "Edit Teams" access permission.

The method `humanresources.node.edit` updates the properties of a department or team.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../../data-types.md) | Identifier of the department or team.

The identifier can be obtained using the [humanresources.node.list](./humanresources-node-list.md) method. ||
|| **name**
[`string`](../../../data-types.md) | New name of the department or team. ||
|| **description**
[`string`](../../../data-types.md) | New description of the department or team. ||
|| **colorName**
[`string`](../../../data-types.md) | New color of the team.

Possible values:

- `blue` — blue
- `green` — green
- `cyan` — cyan
- `orange` — orange
- `purple` — purple
- `pink` — pink ||
|#

{% note info "" %}

Please provide at least one field to change: `name`, `description`, or `colorName`.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` parameter in the request:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/humanresources.node.edit`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":44,"name":"B2B Sales Department","description":"Works with corporate clients"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/humanresources.node.edit
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":44,"name":"B2B Sales Department","description":"Works with corporate clients","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/humanresources.node.edit
    ```

- JS

    The SDK does not yet support calls to the /rest/api/ address. Use direct HTTP requests, such as via curl or fetch.

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'humanresources.node.edit',
            {
                id: 44,
                name: 'B2B Sales Department',
                description: 'Works with corporate clients'
            }
        );

        const result = response.getData().result;
        console.info('Department updated with ID', result.id);
    }
    catch (error)
    {
        console.error(error);
    }
    ```

- PHP

    The SDK does not yet support calls to the /rest/api/ address. Use direct HTTP requests, such as via curl or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'humanresources.node.edit',
                [
                    'id' => 44,
                    'name' => 'B2B Sales Department',
                    'description' => 'Works with corporate clients',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating department: ' . $e->getMessage();
    }
    ```

- BX24.js

    The SDK does not yet support calls to the /rest/api/ address. Use direct HTTP requests, such as via curl or fetch.

    ```js
    BX24.callMethod(
        'humanresources.node.edit',
        {
            id: 44,
            name: 'B2B Sales Department',
            description: 'Works with corporate clients'
        },
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    The SDK does not yet support calls to the /rest/api/ address. Use direct HTTP requests, such as via curl or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'humanresources.node.edit',
        [
            'id' => 44,
            'name' => 'B2B Sales Department',
            'description' => 'Works with corporate clients'
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
        "id": 44,
        "name": "B2B Sales Department",
        "type": "DEPARTMENT",
        "structureId": 1,
        "parentId": 1,
        "description": "Works with corporate clients",
        "accessCode": "DR44",
        "userCount": 18,
        "colorName": null,
        "xmlId": null,
        "createdAt": "2026-05-30T09:20:15+02:00",
        "updatedAt": "2026-06-02T12:40:18+02:00"
    },
    "time": {
        "start": 1780393218,
        "finish": 1780393218.504217,
        "duration": 0.5042169094085693,
        "processing": 0.47100210189819336,
        "date_start": "2026-06-02T12:40:18+02:00",
        "date_finish": "2026-06-02T12:40:18+02:00",
        "operating_reset_at": 1780393818,
        "operating": 0
    }
}
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with data of the updated department or team. ||
|| **id**
[`integer`](../../../data-types.md) | Identifier of the department or team. ||
|| **name**
[`string`](../../../data-types.md) | Name of the department or team. ||
|| **type**
[`string`](../../../data-types.md) | Type of the structure element. ||
|| **structureId**
[`integer`](../../../data-types.md) | Identifier of the company structure. ||
|| **parentId**
[`integer`](../../../data-types.md) | Identifier of the parent department or team. ||
|| **description**
[`string`](../../../data-types.md) | Description of the department or team. ||
|| **accessCode**
[`string`](../../../data-types.md) | Access code of the department or team. ||
|| **userCount**
[`integer`](../../../data-types.md) | Number of users in the department or team. ||
|| **colorName**
[`string`](../../../data-types.md) | Color of the team, if specified. ||
|| **xmlId**
[`string`](../../../data-types.md) | External identifier of the department or team. ||
|| **createdAt**
[`datetime`](../../../data-types.md) | Date and time of creation of the department or team. ||
|| **updatedAt**
[`datetime`](../../../data-types.md) | Date and time of the last update of the department or team. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request. ||
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
                "message": "At least one of the following fields must be provided: name, description, colorName.",
                "field": "name"
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
|| `id` | Required field `id` is not specified. | Add `id` to the request body. ||
|| `id` | Field `id` requires data type `int` for this request. | Ensure that the value of `id` is passed as a number. ||
|| `name`
`description`
`colorName` | At least one field must be provided for modification. | Provide `name`, `description`, or `colorName`. ||
|| `colorName` | An invalid color value was provided. | Use one of the valid values from the `colorName` parameter description. ||
|#

#### Access Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Access denied. | The user does not have permission to edit the specified department or team. ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION`

#| 
|| **Field** | **Error Description** | **How to Fix** ||
|| `-` | Insufficient access rights: required scope is missing. | Check the `humanresources` scope. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./humanresources-node-get.md)
- [{#T}](./humanresources-node-move.md)
- [{#T}](./humanresources-node-add.md)
- [{#T}](./index.md)