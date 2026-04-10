# Get Available User Field Types userfieldconfig.getTypes

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`userfieldconfig`](../../../../scopes/permissions.md), module scope from `moduleId` (for example, [`crm`](../../../../scopes/permissions.md))
>
> Who can execute the method: a user with read access permission to the object that owns the field in the `moduleId`

The method `userfieldconfig.getTypes` returns a set of available user field types for the module.

## Method Parameters

{% include [Parameter Note](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **moduleId***
[`string`](../../../data-types.md) | Identifier of the module for which to retrieve available field types ||
|#

## Code Examples

{% include [Example Note](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"moduleId":"crm"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/userfieldconfig.getTypes
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"moduleId":"crm","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/userfieldconfig.getTypes
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'userfieldconfig.getTypes',
    		{
    			moduleId: 'crm',
    		}
    	);

    	const result = response.getData().result;
    	console.info(result);
    }
    catch (error)
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'userfieldconfig.getTypes',
                [
                    'moduleId' => 'crm',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Result: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'userfieldconfig.getTypes',
        {
            moduleId: 'crm',
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'userfieldconfig.getTypes',
        [
            'moduleId' => 'crm',
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
        "types": {
            "crm": {
                "userTypeId": "crm",
                "description": "Link to CRM elements"
            },
            "crm_status": {
                "userTypeId": "crm_status",
                "description": "Link to CRM directories"
            },
            "money": {
                "userTypeId": "money",
                "description": "Money"
            },
            "employee": {
                "userTypeId": "employee",
                "description": "Link to employee"
            },
            "rest_219_test": {
                "userTypeId": "rest_219_test",
                "description": "Test type"
            },
            "string": {
                "userTypeId": "string",
                "description": "String"
            },
            "integer": {
                "userTypeId": "integer",
                "description": "Integer"
            },
            "double": {
                "userTypeId": "double",
                "description": "Number"
            },
            "datetime": {
                "userTypeId": "datetime",
                "description": "Date with time"
            },
            "date": {
                "userTypeId": "date",
                "description": "Date"
            },
            "boolean": {
                "userTypeId": "boolean",
                "description": "Yes/No"
            },
            "address": {
                "userTypeId": "address",
                "description": "Address"
            },
            "url": {
                "userTypeId": "url",
                "description": "Link"
            },
            "file": {
                "userTypeId": "file",
                "description": "File"
            },
            "enumeration": {
                "userTypeId": "enumeration",
                "description": "List"
            },
            "iblock_section": {
                "userTypeId": "iblock_section",
                "description": "Link to information block sections"
            },
            "iblock_element": {
                "userTypeId": "iblock_element",
                "description": "Link to information block elements"
            }
        }
    },
    "time": {
        "start": 1774356634,
        "finish": 1774356634.880673,
        "duration": 0.8806729316711426,
        "processing": 0,
        "date_start": "2026-03-24T15:50:34+01:00",
        "date_finish": "2026-03-24T15:50:34+01:00",
        "operating_reset_at": 1774357234,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **types**
[`object`](../../../data-types.md) | Dictionary of available types, where the key is the type identifier and the value is an object with fields `userTypeId` and `description` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The current method required more scopes. (crm)"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | The current method required more scopes. (crm) | The application does not have the required scope for the module from `moduleId` ||
|| `-` | No settings for UserFieldAccess | Access to user fields is not configured for the provided `moduleId` ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./userfieldconfig-add.md)
- [{#T}](./userfieldconfig-update.md)
- [{#T}](./userfieldconfig-get.md)
- [{#T}](./userfieldconfig-list.md)
- [{#T}](./userfieldconfig-delete.md)