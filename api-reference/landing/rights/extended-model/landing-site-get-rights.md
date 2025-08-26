# Get access permissions of the current user landing.site.getRights

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.getRights` will return the permissions of the current user. In the case of a non-existent site or lack of permissions for it, the same state will be returned – an empty array. Otherwise, an array consisting of possible values will be returned:

- **denied** - all actions are forbidden,
- **read** – read (permission is automatically granted by the system when any other value except denied is specified),
- **edit** – edit (content of pages),
- **sett** – change settings,
- **public** – publish,
- **delete** – delete (to trash, and restore from trash).

## Parameters

#|
|| **Method** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Site identifier. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.getRights',
    		{
    			id: 645
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
                'landing.site.getRights',
                [
                    'id' => 645,
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
        echo 'Error getting site rights: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.getRights',
        {
            id: 645
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

{% include [Examples note](../../../../_includes/examples.md) %}