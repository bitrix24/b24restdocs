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

The method `crm.deal.details.configuration.get` retrieves the settings of deal cards. The method reads the personal settings of the specified user or the general settings defined for all users.

{% note warning %}

Please note that the settings of deal cards for different categories (or funnels) may differ from each other. 
To switch between the settings of deal cards for different categories, the parameter **dealCategoryId** is used.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **scope**
[`unknown`](../../../data-types.md) | The scope of the settings. Allowed values:

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

- JS

    ```js
    //-- 
    //Request personal settings of deal cards for the user with identifier 1.
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
    //Request general settings of deal cards for the general category.
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
    //Request general settings of deal cards for the category with identifier 1.
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}