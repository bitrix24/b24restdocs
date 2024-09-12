# Reset the parameters of crm.deal.details.configuration.reset

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter mandatory status is not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.details.configuration.reset` resets the settings of deal cards. This method removes the personal settings of the specified user or the general settings defined for all users.

{% note warning %}

Please note that the settings of deal cards for different directions (or funnels) may vary from each other. 
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
[`unknown`](../../../data-types.md) | User identifier. If not specified, the current user is taken. Needed only when resetting personal settings. ||
|| **extras**
[`unknown`](../../../data-types.md) | Additional parameters. Here, for deals, the parameter `dealCategoryId` can be specified. ||
|#

## Examples

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

{% include [Footnote on examples](../../../../_includes/examples.md) %}