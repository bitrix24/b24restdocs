# Get a list of sites with rights for the role landing.role.getRights

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- corrections needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

{% note info "" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Execution rights**: `administrator`

{% endnote %}

The method `landing.role.getRights` allows you to retrieve a list of sites for which rights are set within the role. The method will return an array (see example), where the keys will be the site identifiers, and the values will be arrays of available operations (a zero key indicates default access for the role):

- **denied** - all access is denied,
- **read** – read (the right is automatically granted by the system when any other right besides denied is specified),
- **edit** – modification (of page content),
- **sett** – change settings,
- **public** – publication,
- **delete** – deletion (to the trash, and restoration from the trash).

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id**
[`unknown`](../../../data-types.md) | Role identifier. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.role.getRights',
    		{
    			id: 11
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
                'landing.role.getRights',
                [
                    'id' => 11
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting role rights: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.role.getRights',
        {
            id: 11
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