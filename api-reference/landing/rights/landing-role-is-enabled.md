# Determine the models of landing.role.isEnabled

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `landing.role.isEnabled` determines which model is currently enabled on the project, either the extended or role-based model.

## Parameters

No parameters.

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.role.isEnabled',
    		{}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		if (result.data())
    		{
    			console.log('Role-based model');
    		}
    		else
    		{
    			console.log('Extended model');
    		}
    	}
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.role.isEnabled',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            if ($result->data()) {
                echo 'Role-based model';
            } else {
                echo 'Extended model';
            }
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error checking role model: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.role.isEnabled',
        {
        },
        function(result)
        {
            if(result.error())
            {
                console.error(result.error());
            }
            else
            {
                if (result.data())
                {
                    console.log('Role-based model');
                }
                else
                {
                    console.log('Extended model');
                }
            }
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}