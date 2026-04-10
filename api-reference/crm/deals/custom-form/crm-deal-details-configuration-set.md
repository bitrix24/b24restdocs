# Set Parameters for the Deal Card crm.deal.details.configuration.set

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method:
> - a user can set their personal settings
> - personal settings for another user can be set if the user has editing rights for that user's personal view
> - general settings can be set if the user has editing rights for the common view

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [crm.item.details.configuration.set](../../universal/item-details-configuration/crm-item-details-configuration-set.md).

{% endnote %}

The method `crm.deal.details.configuration.set` sets the settings for the deal card. It records the personal settings for the specified user or the general settings for all users.

{% note info %}

The settings for deal cards in different Sales Funnels may vary. To select a Sales Funnel, use the `extras.dealCategoryId` parameter.

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
- `C` — general settings

Default is `P`
||
|| **userId**
[`user`](../../../data-types.md) | User identifier. Required only when setting personal settings for another user.

If not specified, the current user is used
||
|| **data*** 
[`section[]`](#section) | A list of `section` describing the configuration of the deal card sections. The structure of `section` is described below ||
|| **extras**
[`object`](../../../data-types.md) | Additional parameters [(detailed description)](#parameter-extras) ||
|#

### Extras Parameter {#parameter-extras}

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **dealCategoryId**
[`integer`](../../../data-types.md) | Identifier of the deal funnel. Can be obtained using [crm.category.list](../../universal/category/crm-category-list.md)

If not specified, the default funnel for deals is used
||
|#

### Section Parameter {#section}

Describes an individual section with fields within the deal card

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Unique name of the section ||
|| **title***
[`string`](../../../data-types.md) | Title of the section ||
|| **type***
[`string`](../../../data-types.md) | Type of the section

Currently, only the value `section` is available
||
|| **elements**
[`section_element[]`](#section_element) | List of fields displayed in the card with additional settings ||
|#

### Section Element Parameter {#section_element}

Configuration of an individual field within the section

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../../data-types.md) | Field identifier. A list of available fields can be found using [`crm.deal.fields`](../crm-deal-fields.md) ||
|| **optionFlags**
[`integer`](../../../data-types.md) | Whether to always show the field:
- `1` — yes
- `0` — no

Default is `0`
||
|| **options**
[`object`](../../../data-types.md) | Additional options for the field. The composition depends on the field ||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

Set personal configuration for the deal card for the user with `id = 1` in the funnel with `id = 32`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"scope":"P","userId":1,"extras":{"dealCategoryId":32},"data":[{"name":"main","title":"About the Deal","type":"section","elements":[{"name":"TITLE"},{"name":"OPPORTUNITY_WITH_CURRENCY"},{"name":"STAGE_ID"},{"name":"CLOSEDATE"},{"name":"CLIENT"}]},{"name":"additional","title":"Additional Information","type":"section","elements":[{"name":"TYPE_ID"},{"name":"SOURCE_ID"},{"name":"SOURCE_DESCRIPTION"},{"name":"OPENED"},{"name":"ASSIGNED_BY_ID"},{"name":"COMMENTS"}]},{"name":"products","title":"Products","type":"section","elements":[{"name":"PRODUCT_ROW_SUMMARY"}]}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.details.configuration.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"scope":"P","userId":1,"extras":{"dealCategoryId":32},"data":[{"name":"main","title":"About the Deal","type":"section","elements":[{"name":"TITLE"},{"name":"OPPORTUNITY_WITH_CURRENCY"},{"name":"STAGE_ID"},{"name":"CLOSEDATE"},{"name":"CLIENT"}]},{"name":"additional","title":"Additional Information","type":"section","elements":[{"name":"TYPE_ID"},{"name":"SOURCE_ID"},{"name":"SOURCE_DESCRIPTION"},{"name":"OPENED"},{"name":"ASSIGNED_BY_ID"},{"name":"COMMENTS"}]},{"name":"products","title":"Products","type":"section","elements":[{"name":"PRODUCT_ROW_SUMMARY"}]}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.details.configuration.set
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.deal.details.configuration.set',
    		{
    			scope: "P",
    			userId: 1,
    			extras: {
    				dealCategoryId: 32,
    			},
    			data: [
    				{
    					name: "main",
    					title: "About the Deal",
    					type: "section",
    					elements: [
    						{ name: "TITLE" },
    						{ name: "OPPORTUNITY_WITH_CURRENCY" },
    						{ name: "STAGE_ID" },
    						{ name: "CLOSEDATE" },
    						{ name: "CLIENT" },
    					],
    				},
    				{
    					name: "additional",
    					title: "Additional Information",
    					type: "section",
    					elements: [
    						{ name: "TYPE_ID" },
    						{ name: "SOURCE_ID" },
    						{ name: "SOURCE_DESCRIPTION" },
    						{ name: "OPENED" },
    						{ name: "ASSIGNED_BY_ID" },
    						{ name: "COMMENTS" },
    					],
    				},
    				{
    					name: "products",
    					title: "Products",
    					type: "section",
    					elements: [
    						{ name: "PRODUCT_ROW_SUMMARY" },
    					],
    				},
    			],
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
                'crm.deal.details.configuration.set',
                [
                    'scope'  => 'P',
                    'userId' => 1,
                    'extras' => [
                        'dealCategoryId' => 32,
                    ],
                    'data' => [
                        [
                            'name'     => 'main',
                            'title'    => 'About the Deal',
                            'type'     => 'section',
                            'elements' => [
                                ['name' => 'TITLE'],
                                ['name' => 'OPPORTUNITY_WITH_CURRENCY'],
                                ['name' => 'STAGE_ID'],
                                ['name' => 'CLOSEDATE'],
                                ['name' => 'CLIENT'],
                            ],
                        ],
                        [
                            'name'     => 'additional',
                            'title'    => 'Additional Information',
                            'type'     => 'section',
                            'elements' => [
                                ['name' => 'TYPE_ID'],
                                ['name' => 'SOURCE_ID'],
                                ['name' => 'SOURCE_DESCRIPTION'],
                                ['name' => 'OPENED'],
                                ['name' => 'ASSIGNED_BY_ID'],
                                ['name' => 'COMMENTS'],
                            ],
                        ],
                        [
                            'name'     => 'products',
                            'title'    => 'Products',
                            'type'     => 'section',
                            'elements' => [
                                ['name' => 'PRODUCT_ROW_SUMMARY'],
                            ],
                        ],
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
        echo 'Error setting deal details configuration: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.deal.details.configuration.set',
        {
            scope: "P",
            userId: 1,
            extras: {
                dealCategoryId: 32,
            },
            data: [
                {
                    name: "main",
                    title: "About the Deal",
                    type: "section",
                    elements: [
                        { name: "TITLE" },
                        { name: "OPPORTUNITY_WITH_CURRENCY" },
                        { name: "STAGE_ID" },
                        { name: "CLOSEDATE" },
                        { name: "CLIENT" },
                    ],
                },
                {
                    name: "additional",
                    title: "Additional Information",
                    type: "section",
                    elements: [
                        { name: "TYPE_ID" },
                        { name: "SOURCE_ID" },
                        { name: "SOURCE_DESCRIPTION" },
                        { name: "OPENED" },
                        { name: "ASSIGNED_BY_ID" },
                        { name: "COMMENTS" },
                    ],
                },
                {
                    name: "products",
                    title: "Products",
                    type: "section",
                    elements: [
                        { name: "PRODUCT_ROW_SUMMARY" },
                    ],
                },
            ],
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
        'crm.deal.details.configuration.set',
        [
            'scope' => 'P',
            'userId' => 1,
            'extras' => [
                'dealCategoryId' => 32,
            ],
            'data' => [
                [
                    'name' => 'main',
                    'title' => 'About the Deal',
                    'type' => 'section',
                    'elements' => [
                        ['name' => 'TITLE'],
                        ['name' => 'OPPORTUNITY_WITH_CURRENCY'],
                        ['name' => 'STAGE_ID'],
                        ['name' => 'CLOSEDATE'],
                        ['name' => 'CLIENT'],
                    ],
                ],
                [
                    'name' => 'additional',
                    'title' => 'Additional Information',
                    'type' => 'section',
                    'elements' => [
                        ['name' => 'TYPE_ID'],
                        ['name' => 'SOURCE_ID'],
                        ['name' => 'SOURCE_DESCRIPTION'],
                        ['name' => 'OPENED'],
                        ['name' => 'ASSIGNED_BY_ID'],
                        ['name' => 'COMMENTS'],
                    ],
                ],
                [
                    'name' => 'products',
                    'title' => 'Products',
                    'type' => 'section',
                    'elements' => [
                        ['name' => 'PRODUCT_ROW_SUMMARY'],
                    ],
                ],
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
        "start": 1773311825,
        "finish": 1773311825.969825,
        "duration": 0.969825029373169,
        "processing": 0,
        "date_start": "2026-03-12T13:37:05+01:00",
        "date_finish": "2026-03-12T13:37:05+01:00",
        "operating_reset_at": 1773312425,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response. Returns `true` if the settings were successfully recorded ||
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

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty Value | Access denied | No rights to set deal card settings ||
|| `400` | Empty Value | Parameter 'data' must be array | A non-array was passed in `data` ||
|| `400` | Empty Value | There are no data to write | An empty array was passed in `data` ||
|| `400` | Empty Value | The data must be indexed array | A non-indexed array was passed in `data` ||
|| `400` | Empty Value | Section at index `i` has type `data[i].type`. The expected type is 'section' | The value in `data[i].type` is different from `section` ||
|| `400` | Empty Value | Section at index `i` does not have a name | An empty value was passed in `data[i].name` ||
|| `400` | Empty Value | Section at index `i` does not have a title | An empty value was passed in `data[i].title` ||
|| `400` | Empty Value | Element at index `j` in section at index `i` does not have a name | An empty value was passed in `data[i].elements[j].name` ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-deal-details-configuration-get.md)
- [{#T}](./crm-deal-details-configuration-reset.md)
- [{#T}](./crm-deal-details-configuration-force-common-scope-for-all.md)