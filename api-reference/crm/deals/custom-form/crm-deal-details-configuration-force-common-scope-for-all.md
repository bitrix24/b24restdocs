# Set a Common Card for All Users crm.deal.details.configuration.forceCommonScopeForAll

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method:
> - A common card can be set if the user has editing rights for the common view.

{% note warning "Method Development Halted" %}

The method `crm.deal.details.configuration.forceCommonScopeForAll` continues to function, but there is a more relevant alternative: [crm.item.details.configuration.forceCommonScopeForAll](../../universal/item-details-configuration/crm-item-details-configuration-forceCommonScopeForAll.md).

{% endnote %}

The method `crm.deal.details.configuration.forceCommonScopeForAll` forcibly sets a common deal card for all users and removes their personal settings.

{% note info %}

The settings for deal cards in different Sales Funnels may vary. To select a funnel, use the `extras.dealCategoryId` parameter.

{% endnote %}

## Method Parameters

{% include [Parameter Notes](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **extras**
[`object`](../../../data-types.md) | Additional parameters [(detailed description)](#parameter-extras) ||
|#

### extras Parameter {#parameter-extras}

{% include [Parameter Notes](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **dealCategoryId**
[`integer`](../../../data-types.md) | Identifier of the deal funnel. Can be obtained using [crm.category.list](../../universal/category/crm-category-list.md)

If not specified, the default funnel for deals is used.
||
|#

## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

Set a common deal card for all users in the funnel with `id = 32`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"extras":{"dealCategoryId":32}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.details.configuration.forceCommonScopeForAll
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"extras":{"dealCategoryId":32},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.details.configuration.forceCommonScopeForAll
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.deal.details.configuration.forceCommonScopeForAll',
    		{
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
                'crm.deal.details.configuration.forceCommonScopeForAll',
                [
                    'extras' => [
                        'dealCategoryId' => 32,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting common deal card for all users: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.deal.details.configuration.forceCommonScopeForAll',
        {
            extras: {
                dealCategoryId: 32,
            },
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
        'crm.deal.details.configuration.forceCommonScopeForAll',
        [
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
        "start": 1773312463,
        "finish": 1773312463.299701,
        "duration": 0.2997009754180908,
        "processing": 0,
        "date_start": "2026-03-12T13:47:43+01:00",
        "date_finish": "2026-03-12T13:47:43+01:00",
        "operating_reset_at": 1773313063,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response. Returns `true` on success ||
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
|| `400` | Empty Value | Access denied | No rights to set a common card for all users ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-deal-details-configuration-get.md)
- [{#T}](./crm-deal-details-configuration-set.md)
- [{#T}](./crm-deal-details-configuration-reset.md)