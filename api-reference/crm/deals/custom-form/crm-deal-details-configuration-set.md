# Set Parameters for the Individual Card crm.deal.details.configuration.set

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.details.configuration.set` allows you to set the settings for deal cards. The method records the personal settings of the specified user’s card or general settings for all users.

{% note warning %}

Please note that the settings for deal cards of different directions (or funnels) may differ from each other. 
To switch between the settings of deal cards of different directions, the parameter **dealCategoryId** is used.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **scope**
[`unknown`](../../../data-types.md) | The scope of the settings. Acceptable values:

- **P** - personal settings,
- **C** - general settings.
 ||
|| **userId**
[`unknown`](../../../data-types.md) | User identifier. If not specified, the current one is used. Required only when setting personal settings. ||
|| **extras**
[`unknown`](../../../data-types.md) | Additional parameters. Here, for deals, the parameter `dealCategoryId` can be specified. ||
|#

## Examples

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.deal.details.configuration.set",
    		{
    			scope: "P",
    			userId: 1,
    			data:
    			[
    				{
    					name: "main",
    					title: "About the Deal",
    					type: "section",
    					elements:
    					[
    						{ name: "TITLE" },
    						{ name: "OPPORTUNITY_WITH_CURRENCY" },
    						{ name: "STAGE_ID" },
    						{ name: "BEGINDATE" },
    						{ name: "CLOSEDATE" },
    						{ name: "CLIENT" }
    					]
    				},
    				{
    					name: "additional",
    					title: "Additional Information",
    					type: "section",
    					elements:
    					[
    						{ name: "TYPE_ID" },
    						{ name: "SOURCE_ID" },
    						{ name: "SOURCE_DESCRIPTION" },
    						{ name: "OPENED" },
    						{ name: "ASSIGNED_BY_ID" },
    						{ name: "OBSERVER" },
    						{ name: "COMMENTS" }
    					]
    				},
    				{
    					name: "products",
    					title: "Products",
    					type: "section",
    					elements:
    					[
    						{ name: "PRODUCT_ROW_SUMMARY" }
    					]
    				}
    			]
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		console.error(result.error());
    	else
    		console.dir(result);
    }
    catch(error)
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
                    'data'   => [
                        [
                            'name'     => 'main',
                            'title'    => 'About the Deal',
                            'type'     => 'section',
                            'elements' => [
                                ['name' => 'TITLE'],
                                ['name' => 'OPPORTUNITY_WITH_CURRENCY'],
                                ['name' => 'STAGE_ID'],
                                ['name' => 'BEGINDATE'],
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
                                ['name' => 'OBSERVER'],
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
    
        echo 'Success: ' . print_r($result, true);
        console.dir($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting deal details configuration: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    //---
    //Setting personal settings for the general direction deal card for the user with identifier 1.
    BX24.callMethod(
        "crm.deal.details.configuration.set",
        {
            scope: "P",
            userId: 1,
            data:
            [
                {
                    name: "main",
                    title: "About the Deal",
                    type: "section",
                    elements:
                    [
                        { name: "TITLE" },
                        { name: "OPPORTUNITY_WITH_CURRENCY" },
                        { name: "STAGE_ID" },
                        { name: "BEGINDATE" },
                        { name: "CLOSEDATE" },
                        { name: "CLIENT" }
                    ]
                },
                {
                    name: "additional",
                    title: "Additional Information",
                    type: "section",
                    elements:
                    [
                        { name: "TYPE_ID" },
                        { name: "SOURCE_ID" },
                        { name: "SOURCE_DESCRIPTION" },
                        { name: "OPENED" },
                        { name: "ASSIGNED_BY_ID" },
                        { name: "OBSERVER" },
                        { name: "COMMENTS" }
                    ]
                },
                {
                    name: "products",
                    title: "Products",
                    type: "section",
                    elements:
                    [
                        { name: "PRODUCT_ROW_SUMMARY" }
                    ]
                }
            ]
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    //---
    ```

{% endlist %}

{% include [Examples Note](../../../../_includes/examples.md) %}