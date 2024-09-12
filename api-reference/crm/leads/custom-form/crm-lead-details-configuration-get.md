# Get Parameters of crm.lead.details.configuration.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
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

The method `crm.lead.details.configuration.get` retrieves the configuration parameters for lead detail cards. This method reads the personal settings of the specified user or the general settings defined for all users.

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
[`unknown`](../../../data-types.md) | User identifier. If not specified, the current user is taken. Required only when requesting personal settings. ||
|| **extras**
[`unknown`](../../../data-types.md) | Additional parameters. Here, for leads, the parameter `leadCustomerType` can be specified, with allowed values:

- **1** - simple leads,
- **2** - repeat leads.
  ||
|#

## Examples

```js
//--
//Request personal settings for the lead card for the user with identifier 1.
BX24.callMethod(
    "crm.lead.details.configuration.get",
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
//Request general settings for the lead card.
BX24.callMethod(
    "crm.lead.details.configuration.get",
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
//Request general settings for the repeat lead card.
BX24.callMethod(
    "crm.lead.details.configuration.get",
    {
        scope: "C",
        extras: { leadCustomerType: 2 }
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