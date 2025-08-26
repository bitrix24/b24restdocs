# Get Page ID by URL landing.landing.resolveIdByPublicUrl

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.landing.resolveIdByPublicUrl` returns the page ID based on the provided relative URL of the page.

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **landingUrl**
[`unknown`](../../../data-types.md) | Relative URL of the page. ||
|| **siteId**
[`unknown`](../../../data-types.md) | Site ID. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.landing.resolveIdByPublicUrl',
    		{
    			landingUrl: '/folder/sub/folder/page/',
    			siteId: 1817
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
                'landing.landing.resolveIdByPublicUrl',
                [
                    'landingUrl' => '/folder/sub/folder/page/',
                    'siteId'     => 1817
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Info: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error resolving landing ID: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.resolveIdByPublicUrl',
        {
            landingUrl: '/folder/sub/folder/page/',
            siteId: 1817
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

{% include [Example notes](../../../../_includes/examples.md) %}