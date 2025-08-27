# Get parameters for crm.company.details.configuration.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for standard writing
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.company.details.configuration.get` retrieves the settings for company cards. The method reads the personal settings of the specified user or the general settings defined for all users.

#|
|| **Parameter** | **Description** ||
|| **scope**
[`unknown`](../../../data-types.md) | The scope of the settings. Allowed values:

- **P** - personal settings,
- **C** - common settings.
 ||
|| **userId**
[`unknown`](../../../data-types.md) | User identifier. If not specified, the current one is used. Required only when requesting personal settings. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response1 = await $b24.callMethod(
    		"crm.company.details.configuration.get",
    		{
    			scope: "P",
    			userId: 1
    		}
    	);
    	
    	const result1 = response1.getData().result;
    	console.dir(result1);
    	
    	const response2 = await $b24.callMethod(
    		"crm.company.details.configuration.get",
    		{
    			scope: "C"
    		}
    	);
    	
    	const result2 = response2.getData().result;
    	console.dir(result2);
    }
    catch(error)
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        //Request personal settings for the company card for the user with identifier 1.
        $response1 = $b24Service
            ->core
            ->call(
                'crm.company.details.configuration.get',
                [
                    'scope'  => 'P',
                    'userId' => 1,
                ]
            );
    
        $result1 = $response1
            ->getResponseData()
            ->getResult();
    
        echo 'Personal company details configuration for user 1: ' . print_r($result1, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting personal company details configuration: ' . $e->getMessage();
    }
    
    try {
        //Request common settings for the company card.
        $response2 = $b24Service
            ->core
            ->call(
                'crm.company.details.configuration.get',
                [
                    'scope' => 'C',
                ]
            );
    
        $result2 = $response2
            ->getResponseData()
            ->getResult();
    
        echo 'Common company details configuration: ' . print_r($result2, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting common company details configuration: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    //--
    //Request personal settings for the company card for the user with identifier 1.
    BX24.callMethod(
        "crm.company.details.configuration.get",
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
    //Request common settings for the company card.
    BX24.callMethod(
        "crm.company.details.configuration.get",
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
    //---
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}