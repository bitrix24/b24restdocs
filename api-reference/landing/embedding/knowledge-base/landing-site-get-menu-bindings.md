# Get the list of bindings in the method landing.site.getMenuBindings

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.getMenuBindings` returns a list of bindings associated with the menu (specific or all) Knowledge Bases. Only the bindings to the Knowledge Bases that the current user has read access to will be returned.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **menuCode**
[`unknown`](../../../data-types.md) | Symbolic code of the menu, as defined above. Optional, by default all bindings are returned. | ||
|#

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.getMenuBindings',
    		{
    			menuCode: 'crm_switcher:deal'
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
                'landing.site.getMenuBindings',
                [
                    'menuCode' => 'crm_switcher:deal'
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting menu bindings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.getMenuBindings',
        {
            menuCode: 'crm_switcher:deal'
        },
        function(result)
        {
            if(result.error())
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

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}