# Reset Deal Card Settings crm.deal.details.configuration.reset

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

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "Method Development Stopped" %}

The method `crm.deal.details.configuration.reset` continues to function, but there is a more relevant alternative [crm.item.details.configuration.reset](../../universal/item-details-configuration/crm-item-details-configuration-reset.md).

{% endnote %}

The method `crm.deal.details.configuration.reset` resets the settings of deal cards. It removes personal settings for the specified user or the general settings defined for all users.

{% note warning %}

Please note that the settings for deal cards of different directions (or funnels) may differ from each other. 
To switch between the settings of deal cards of different directions, the parameter **dealCategoryId** is used.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **scope**
[`unknown`](../../../data-types.md) | The scope of the settings. Allowed values:

- **P** - personal settings,
- **C** - general settings.
 ||
|| **userId**
[`unknown`](../../../data-types.md) | User identifier. If not specified, the current user is taken. Needed only when resetting personal settings. ||
|| **extras**
[`unknown`](../../../data-types.md) | Additional parameters. Here, the parameter `dealCategoryId` can be specified for deals. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.deal.details.configuration.reset",
    		{
    			scope: "P",
    			userId: 1
    		}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
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
                'crm.deal.details.configuration.reset',
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
        echo 'Error resetting deal details configuration: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    //---
    //Reset personal settings of the general direction deal card for the user with identifier 1.
    BX24.callMethod(
        "crm.deal.details.configuration.reset",
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

{% include [Examples Note](../../../../_includes/examples.md) %}