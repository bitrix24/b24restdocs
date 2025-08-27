# Reset Lead Card Settings crm.lead.details.configuration.reset

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.lead.details.configuration.reset` resets the settings of lead cards. It removes the personal settings of the specified user’s card or the general settings defined for all users.

{% note warning %}

Please note that the settings for repeat leads may differ from those for simple leads. The parameter **leadCustomerType** is used to switch between lead card settings.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **scope**
[`unknown`](../../../data-types.md) | The scope of the settings. Allowed values:

- **P** - personal settings,
- **C** - general settings.
 ||
|| **userId**
[`unknown`](../../../data-types.md) | User identifier. If not specified, the current user is taken. Required only when resetting personal settings. ||
|| **extras**
[`unknown`](../../../data-types.md) | Additional parameters. Here, for leads, the parameter `leadCustomerType` can be specified, with allowed values:

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
    		"crm.lead.details.configuration.reset",
    		{
    			scope: "P",
    			userId: 1
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.dir(result);
    	}
    }
    catch(error)
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
                'crm.lead.details.configuration.reset',
                [
                    'scope'  => 'P',
                    'userId' => 1,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error resetting lead details configuration: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    //---
    //Reset personal settings of the lead card for the user with identifier 1.
    BX24.callMethod(
        "crm.lead.details.configuration.reset",
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
    //---
    ```

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}