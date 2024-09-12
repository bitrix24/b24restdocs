# Set a Common Detail Form for All Users crm.deal.details.configuration.forceCommonScopeForAll

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.details.configuration.forceCommonScopeForAll` forcibly sets a common detail form for deals for all users.

{% note warning %}

Please note that the settings for deal detail forms of different directions (or Sales Funnels) may vary from each other. 
To switch between the settings of deal detail forms of different directions, the parameter **dealCategoryId** is used.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **extras**
[`unknown`](../../../data-types.md) | Additional parameters. Here, the parameter `dealCategoryId` can be specified for deals. ||
|#

## Examples

```js
//---
//Set a common detail form for general deals for all users.
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}