# Get a List of Available Application Embed Locations placement.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../scopes/permissions.md)
>
> Who can execute the method: a user authorized in the application

The method `placement.list` returns a list of available embed locations for the application.

{% note info "" %}

The method works only in the context of the [application](../../settings/app-installation/index.md).

{% endnote %}

## Method Parameters

{% include [Parameter Note](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **SCOPE**
[`string`](../data-types.md) | Limits the list of embed locations to a specific application scope.

If the parameter is provided and not empty, the method returns embed locations only for the specified scope. ||
|| **FULL**
[`boolean`](../data-types.md) | Flag to retrieve the full list of embed locations.

If the parameter is not provided or is set to `false`, the method returns embed locations for the current application's scope and global embed locations.

If the parameter is set to `true`, the method returns embed locations for all service scopes.

The parameter is considered only if `SCOPE` is not provided. ||
|#

## Code Examples

{% include [Examples Note](../../_includes/examples.md) %}

Example of retrieving a list of embed locations available for the application, where:
- `SCOPE` — the application's scope for which embed locations are needed.

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "SCOPE": "crm",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/placement.list.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'placement.list',
    		{
    			SCOPE: 'crm'
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
                'placement.list',
                [
                    'SCOPE' => 'crm',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting placement list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'placement.list',
        {
            SCOPE: 'crm'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'placement.list',
        [
            'SCOPE' => 'crm',
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        "CRM_DEAL_LIST_TOOLBAR",
        "CRM_LEAD_LIST_TOOLBAR",
        "CRM_CONTACT_LIST_TOOLBAR",
        "CRM_COMPANY_LIST_TOOLBAR",
        "CRM_INVOICE_LIST_TOOLBAR",
        "CRM_QUOTE_LIST_TOOLBAR",
        "CRM_ORDER_LIST_TOOLBAR",
        "CRM_DYNAMIC_136_LIST_TOOLBAR",
        "CRM_DYNAMIC_1038_LIST_TOOLBAR",
        "CRM_SMART_INVOICE_LIST_TOOLBAR",
        "CRM_DEAL_DETAIL_TOOLBAR",
        "CRM_LEAD_DETAIL_TOOLBAR",
        "CRM_CONTACT_DETAIL_TOOLBAR",
        "CRM_COMPANY_DETAIL_TOOLBAR",
        "CRM_INVOICE_DETAIL_TOOLBAR",
        "CRM_QUOTE_DETAIL_TOOLBAR",
        "CRM_DYNAMIC_136_DETAIL_TOOLBAR",
        "CRM_DYNAMIC_1038_DETAIL_TOOLBAR",
        "CRM_SMART_INVOICE_DETAIL_TOOLBAR",
        "CRM_DEAL_ACTIVITY_TIMELINE_MENU",
        "CRM_LEAD_ACTIVITY_TIMELINE_MENU",
        "CRM_QUOTE_ACTIVITY_TIMELINE_MENU"
    ],
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`string[]`](../data-types.md) | A list of codes for embed locations available for the application.

Each element of the array is a string code for an embed location. ||
|| **time**
[`time`](../data-types.md#time) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Current authorization type is denied for this method Application context required"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | Method call not from application context. ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./placements.md)
- [{#T}](./placement-bind.md)
- [{#T}](./placement-get.md)
- [{#T}](./placement-unbind.md)
- [{#T}](./ui-interaction/index.md)