# Get Parameters of crm.deal.details.configuration.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.details.configuration.get` retrieves the settings of deal cards. This method reads the personal settings of the specified user or the general settings defined for all users.

{% note warning %}

Please note that the settings for deal cards of different directions (or Sales Funnels) may vary from each other. 
To switch between the settings of deal cards for different directions, the parameter **dealCategoryId** is used.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **scope**
[`unknown`](../../../data-types.md) | The scope of the settings. Allowed values:

- **P** - personal settings,
- **C** - general settings.
 ||
|| **userId**
[`unknown`](../../../data-types.md) | User identifier. If not specified, the current user is taken. Required only when requesting personal settings. ||
|| **extras**
[`unknown`](../../../data-types.md) | Additional parameters. Here, the parameter `dealCategoryId` can be specified for deals. ||
|#

## Examples

```js
//--
//Request personal settings of the deal card for the user with identifier 1.
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
//Request general settings of the deal card for the general direction.
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
//Request general settings of the deal card for the direction with identifier 1.
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}