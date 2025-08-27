# Set Parameters for crm.lead.details.configuration.set

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for standard writing
- parameter types not specified
- parameter mandatory status not indicated
- examples missing
- success response not provided
- error response not provided

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.lead.details.configuration.set` sets the lead card settings. This method records the personal settings of the specified user’s card or the general settings for all users.

{% note warning %}

Please note that the settings for repeat leads may differ from the settings for simple leads. The parameter **leadCustomerType** is used to switch between lead card settings.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **scope**
[`unknown`](../../../data-types.md) | The scope of the settings. Allowed values:

- **P** - personal settings,
- **C** - general settings.
 ||
|| **userId**
[`unknown`](../../../data-types.md) | User identifier. If not specified, the current user is used. Required only when setting personal settings. ||
|| **extras**
[`unknown`](../../../data-types.md) | Additional parameters. Here, the parameter `leadCustomerType` can be specified for leads, with allowed values:

- **1** - simple leads,
- **2** - repeat leads.
 ||
|#

## Examples

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.lead.details.configuration.set",
    		{
    			scope: "P",
    			userId: 1,
    			data:
    			[
    				{
    					name: "main",
    					title: "General Information",
    					type: "section",
    					elements:
    					[
    						{ name: "TITLE" },
    						{ name: "STATUS_ID" },
    						{ name: "NAME" },
    						{ name: "BIRTHDATE" },
    						{ name: "POST" },
    						{ name: "PHONE" },
    						{ name: "EMAIL" }
    					]
    				},
    				{
    					name: "additional",
    					title: "Additional Information",
    					type: "section",
    					elements:
    					[
    						{ name: "SOURCE_ID" },
    						{ name: "SOURCE_DESCRIPTION" },
    						{ name: "OPENED" },
    						{ name: "ASSIGNED_BY_ID" },
    						{ name: "OBSERVER" },
    						{ name: "COMMENTS" }
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
                'crm.lead.details.configuration.set',
                [
                    'scope' => 'P',
                    'userId' => 1,
                    'data' => [
                        [
                            'name' => 'main',
                            'title' => 'General Information',
                            'type' => 'section',
                            'elements' => [
                                ['name' => 'TITLE'],
                                ['name' => 'STATUS_ID'],
                                ['name' => 'NAME'],
                                ['name' => 'BIRTHDATE'],
                                ['name' => 'POST'],
                                ['name' => 'PHONE'],
                                ['name' => 'EMAIL'],
                            ],
                        ],
                        [
                            'name' => 'additional',
                            'title' => 'Additional Information',
                            'type' => 'section',
                            'elements' => [
                                ['name' => 'SOURCE_ID'],
                                ['name' => 'SOURCE_DESCRIPTION'],
                                ['name' => 'OPENED'],
                                ['name' => 'ASSIGNED_BY_ID'],
                                ['name' => 'OBSERVER'],
                                ['name' => 'COMMENTS'],
                            ],
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting lead details configuration: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    //---
    //Setting personal lead card settings for the user with identifier 1.
    BX24.callMethod(
        "crm.lead.details.configuration.set",
        {
            scope: "P",
            userId: 1,
            data:
            [
                {
                    name: "main",
                    title: "General Information",
                    type: "section",
                    elements:
                    [
                        { name: "TITLE" },
                        { name: "STATUS_ID" },
                        { name: "NAME" },
                        { name: "BIRTHDATE" },
                        { name: "POST" },
                        { name: "PHONE" },
                        { name: "EMAIL" }
                    ]
                },
                {
                    name: "additional",
                    title: "Additional Information",
                    type: "section",
                    elements:
                    [
                        { name: "SOURCE_ID" },
                        { name: "SOURCE_DESCRIPTION" },
                        { name: "OPENED" },
                        { name: "ASSIGNED_BY_ID" },
                        { name: "OBSERVER" },
                        { name: "COMMENTS" }
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