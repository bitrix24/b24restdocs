# Get parameters of crm.lead.details.configuration.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.lead.details.configuration.get` retrieves the configuration parameters for lead cards. The method reads the personal settings of the specified user’s card or the general settings defined for all users.

{% note warning %}

Please note that the settings for repeated lead cards may differ from those for simple lead cards. The parameter **leadCustomerType** is used to switch between lead card settings.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **scope**
[`unknown`](../../../data-types.md) | The scope of the settings. Allowed values:

- **P** - personal settings,
- **C** - common settings. 
  ||
|| **userId**
[`unknown`](../../../data-types.md) | User identifier. If not specified, the current user is taken. Required only when requesting personal settings. ||
|| **extras**
[`unknown`](../../../data-types.md) | Additional parameters. Here, the parameter `leadCustomerType` can be specified for leads, with allowed values:

- **1** - simple leads,
- **2** - repeated leads.
  ||
|#

## Examples

{% list tabs %}

- JS


    ```js
    try
    {
    	const response1 = await $b24.callMethod(
    		"crm.lead.details.configuration.get",
    		{
    			scope: "P",
    			userId: 1
    		}
    	);
    	
    	const result1 = response1.getData().result;
    	console.dir(result1);
    	
    	const response2 = await $b24.callMethod(
    		"crm.lead.details.configuration.get",
    		{
    			scope: "C"
    		}
    	);
    	
    	const result2 = response2.getData().result;
    	console.dir(result2);
    	
    	const response3 = await $b24.callMethod(
    		"crm.lead.details.configuration.get",
    		{
    			scope: "C",
    			extras: { leadCustomerType: 2 }
    		}
    	);
    	
    	const result3 = response3.getData().result;
    	console.dir(result3);
    }
    catch(error)
    {
    	console.error(error);
    }
    ```

- PHP


    ```php
    try {
        //Request personal settings for lead cards for the user with identifier 1.
        $response1 = $b24Service
            ->core
            ->call(
                'crm.lead.details.configuration.get',
                [
                    'scope'  => 'P',
                    'userId' => 1,
                ]
            );
    
        $result1 = $response1
            ->getResponseData()
            ->getResult();
    
        echo 'Personal lead details configuration for user 1: ' . print_r($result1, true);
    
        //Request common settings for lead cards.
        $response2 = $b24Service
            ->core
            ->call(
                'crm.lead.details.configuration.get',
                [
                    'scope' => 'C',
                ]
            );
    
        $result2 = $response2
            ->getResponseData()
            ->getResult();
    
        echo 'Common lead details configuration: ' . print_r($result2, true);
    
        //Request common settings for repeated lead cards.
        $response3 = $b24Service
            ->core
            ->call(
                'crm.lead.details.configuration.get',
                [
                    'scope'  => 'C',
                    'extras' => ['leadCustomerType' => 2],
                ]
            );
    
        $result3 = $response3
            ->getResponseData()
            ->getResult();
    
        echo 'Common lead details configuration for repeated leads: ' . print_r($result3, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching lead details configuration: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    //-- 
    //Request personal settings for lead cards for the user with identifier 1.
    BX24.callMethod(
        "crm.lead.details.configuration.get",
        {
            scope: "P",
            userId: 1
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    //Request common settings for lead cards.
    BX24.callMethod(
        "crm.lead.details.configuration.get",
        {
            scope: "C"
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    //Request common settings for repeated lead cards.
    BX24.callMethod(
        "crm.lead.details.configuration.get",
        {
            scope: "C",
            extras: { leadCustomerType: 2 }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    //-- 
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}