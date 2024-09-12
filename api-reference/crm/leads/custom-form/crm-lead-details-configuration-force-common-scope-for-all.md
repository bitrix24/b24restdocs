# Set a Common Detail Form for All Users crm.lead.details.configuration.forceCommonScopeForAll

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

The method `crm.lead.details.configuration.forceCommonScopeForAll` forcibly sets a common lead detail form for all users.

{% note warning %}

Please note that the settings for repeat leads may differ from the settings for simple leads. The parameter **leadCustomerType** is used to switch between lead detail form settings.

{% endnote %}

#|
|| **Parameter** | **Description** ||
|| **extras**
[`unknown`](../../../data-types.md) | Additional parameters. Here, for leads, the parameter `leadCustomerType` can be specified with the following acceptable values:

- **1** - simple leads,
- **2** - repeat leads 
  ||
|#

## Examples

```js
//---
//Set a common lead detail form for all users.
BX24.callMethod(
    "crm.lead.details.configuration.forceCommonScopeForAll",
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