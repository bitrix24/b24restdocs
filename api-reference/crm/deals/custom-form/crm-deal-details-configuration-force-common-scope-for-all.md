# Set Common Deal Card for All Users crm.deal.details.configuration.forceCommonScopeForAll

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

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

The method `crm.deal.details.configuration.forceCommonScopeForAll` forcibly sets a common deal card for all users.

{% note warning %}

Please note that the settings for deal cards of different categories (or funnels) may vary from each other. 
To switch between the settings of deal cards for different categories, the parameter **dealCategoryId** is used.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **extras**
[`unknown`](../../../data-types.md) | Additional parameters. Here, the parameter `dealCategoryId` can be specified for deals. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    //---
    //Set a common deal card for all users in the general category.
    BX24.callMethod(
        "crm.deal.details.configuration.forceCommonScopeForAll",
        {},
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