# Get parameters of crm.deal.details.configuration.get

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

{% note warning "Method Development Stopped" %}

The method `crm.deal.details.configuration.get` continues to function, but there is a more relevant alternative [crm.item.details.configuration.get](../../universal/item-details-configuration/crm-item-details-configuration-get.md).

{% endnote %}

The method `crm.deal.details.configuration.get` retrieves the settings for deal cards. It reads the personal settings of the specified user or the general settings defined for all users.

{% note warning %}

Please note that the settings for deal cards of different directions (or funnels) may differ from each other. 
To switch between the settings of deal cards for different directions, the parameter **dealCategoryId** is used.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **scope**
[`unknown`](../../../data-types.md) | The scope of the settings. Acceptable values:

- **P** - personal settings,
- **C** - general settings.
 ||
|| **userId**
[`unknown`](../../../data-types.md) | User identifier. If not specified, the current one is used. Required only when requesting personal settings. ||
|| **extras**
[`unknown`](../../../data-types.md) | Additional parameters. Here, the parameter `dealCategoryId` can be specified for deals. ||
|#

## Examples

{% list tabs %}

- PHP

    ```php
    try {
        //Request personal settings for deal cards for the user with ID 1.
        $response1 = $b24Service
            ->core
            ->call(
                "crm.deal.details.configuration.get",
                [
                    'scope'  => "P",
                    'userId' => 1
                ]
            );
    
        $result1 = $response1
            ->getResponseData()
            ->getResult();
    
        echo 'Personal deal details configuration for user 1: ' . print_r($result1, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting personal deal details configuration: ' . $e->getMessage();
    }
    
    try {
        //Request general settings for deal cards for the general direction.
        $response2 = $b24Service
            ->core
            ->call(
                "crm.deal.details.configuration.get",
                [
                    'scope' => "C"
                ]
            );
    
        $result2 = $response2
            ->getResponseData()
            ->getResult();
    
        echo 'Common deal details configuration for general direction: ' . print_r($result2, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting common deal details configuration: ' . $e->getMessage();
    }
    
    try {
        //Request general settings for deal cards for the direction with ID 1.
        $response3 = $b24Service
            ->core
            ->call(
                "crm.deal.details.configuration.get",
                [
                    'scope'  => "C",
                    'extras' => ['dealCategoryId' => 1]
                ]
            );
    
        $result3 = $response3
            ->getResponseData()
            ->getResult();
    
        echo 'Common deal details configuration for direction with ID 1: ' . print_r($result3, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting common deal details configuration for direction with ID 1: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    //--
    //Request personal settings for deal cards for the user with ID 1.
    BX24.callMethod(
        "crm.deal.details.configuration.get",
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
    //Request general settings for deal cards for the general direction.
    BX24.callMethod(
        "crm.deal.details.configuration.get",
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
    //Request general settings for deal cards for the direction with ID 1.
    BX24.callMethod(
        "crm.deal.details.configuration.get",
        {
            scope: "C",
            extras: { dealCategoryId: 1 }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    //--
    ```

{% endlist %}

{% include [Examples note](../../../../_includes/examples.md) %}