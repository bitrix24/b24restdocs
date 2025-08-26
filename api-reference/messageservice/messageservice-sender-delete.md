# Delete SMS provider or messageservice.sender.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- no success response is provided
- no error response is provided
- No examples in other languages

{% endnote %}

{% endif %}

> Scope: [`messageservice`](../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a previously registered message provider. You cannot delete a provider registered by another application or another webhook.

#|
|| **Parameter** | **Description** ||
|| **CODE** | Provider identifier. ||
|#

{% include [Footnote about parameters](../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'messageservice.sender.delete',
    		{
    			'CODE': provider
    		}
    	);
    	
    	const result = response.getData().result;
    	alert("Successfully: " + result);
    }
    catch( error )
    {
    	alert('Error: ' + error);
    }
    ```

- PHP

    ```php
    function uninstallProvider($provider)
    {
        try {
            $response = $b24Service
                ->core
                ->call(
                    'messageservice.sender.delete',
                    [
                        'CODE' => $provider
                    ]
                );
    
            $result = $response
                ->getResponseData()
                ->getResult();
    
            if ($result->error()) {
                echo 'Error: ' . $result->error();
            } else {
                echo 'Successfully: ' . $result->data();
            }
    
        } catch (Throwable $e) {
            error_log($e->getMessage());
            echo 'Error calling messageservice.sender.delete: ' . $e->getMessage();
        }
    }
    ```

- BX24.js

    ```js
    function uninstallProvider(provider)
    {
        BX24.callMethod(
            'messageservice.sender.delete',
            {
                'CODE': provider
            },
            function(result)
            {
                if(result.error())
                    alert('Error: ' + result.error());
                else
                    alert("Successfully: " + result.data());
            }
        );
    }
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}