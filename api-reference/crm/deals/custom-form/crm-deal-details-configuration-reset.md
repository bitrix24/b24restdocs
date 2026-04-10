# Reset Deal Card Settings crm.deal.details.configuration.reset

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method:
> - any user can reset their personal settings
> - resetting another user's personal settings is possible if the user has edit rights for that user's personal view
> - resetting common settings is possible if the user has edit rights for the common view

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.item.details.configuration.reset](../../universal/item-details-configuration/crm-item-details-configuration-reset.md).

{% endnote %}

The method `crm.deal.details.configuration.reset` resets the settings of the deal card. It removes the personal settings of the specified user or the common settings defined for all users.

{% note info %}

The settings of deal cards in different Sales Funnels may vary. To select a Sales Funnel, use the `extras.dealCategoryId` parameter.

{% endnote %}

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | The scope of the settings.

Possible values:
- `P` — personal settings
- `C` — common settings

Default is `P`
||
|| **userId**
[`user`](../../../data-types.md) | User identifier. Required only when resetting another user's personal settings.

If not specified, the current user is used
||
|| **extras**
[`object`](../../../data-types.md) | Additional parameters [(detailed description)](#parameter-extras) ||
|#

### Extras Parameter {#parameter-extras}

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **dealCategoryId**
[`integer`](../../../data-types.md) | Identifier of the Sales Funnel. Can be obtained using [crm.category.list](../../universal/category/crm-category-list.md)

If not specified, the default Sales Funnel for deals is used
||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

Reset the personal configuration of the deal card for the user with `id = 1` in the Sales Funnel with `id = 32`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"scope":"P","userId":1,"extras":{"dealCategoryId":32}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.details.configuration.reset
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"scope":"P","userId":1,"extras":{"dealCategoryId":32},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.details.configuration.reset
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.deal.details.configuration.reset',
    		{
    			scope: "P",
    			userId: 1,
    			extras: {
    				dealCategoryId: 32,
    			},
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
    }
    catch( error )
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
                'crm.deal.details.configuration.reset',
                [
                    'scope'  => 'P',
                    'userId' => 1,
                    'extras' => [
                        'dealCategoryId' => 32,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error resetting deal details configuration: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.deal.details.configuration.reset',
        {
            scope: "P",
            userId: 1,
            extras: {
                dealCategoryId: 32,
            },
        },
        (result) => {
            if (result.error())
            {
                console.error(result.error());

                return;
            }

            console.info(result.data());
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.deal.details.configuration.reset',
        [
            'scope' => 'P',
            'userId' => 1,
            'extras' => [
                'dealCategoryId' => 32,
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
    "result": true,
    "time": {
        "start": 1773307850,
        "finish": 1773307850.136405,
        "duration": 0.13640499114990234,
        "processing": 0,
        "date_start": "2026-03-12T12:30:50+01:00",
        "date_finish": "2026-03-12T12:30:50+01:00",
        "operating_reset_at": 1773308450,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response. Returns `true` if the settings were successfully reset ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty Value | Access denied | No rights to reset deal card settings ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-deal-details-configuration-get.md)
- [{#T}](./crm-deal-details-configuration-set.md)
- [{#T}](./crm-deal-details-configuration-force-common-scope-for-all.md)