# Get the address of the special page on the site landing.syspage.getSpecialPage

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing parameters or fields
- parameter types not specified
- parameter requirements not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.syspage.getSpecialPage` returns the address of a special page on the site. The example shows how to get a link to the personal section page of the site.

{% list tabs %}

- JS


    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.syspage.getSpecialPage',
    		{
    			siteId: 1391,// Site ID
    			type: 'personal',// Type of special page
    			additional: {// Optional array of additional parameters to be added to the URL
    				SECTION: 'private'
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info(result);
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
                'landing.syspage.getSpecialPage',
                [
                    'siteId' => 1391, // Site ID
                    'type' => 'personal', // Type of special page
                    'additional' => [ // Optional array of additional parameters to be added to the URL
                        'SECTION' => 'private'
                    ]
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
        echo 'Error getting special page: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.syspage.getSpecialPage',
        {
            siteId: 1391,// Site ID
            type: 'personal',// Type of special page
            additional: {// Optional array of additional parameters to be added to the URL
                SECTION: 'private'
            }
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