# Reset Company Card Parameters crm.company.details.configuration.reset

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
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

The method `crm.company.details.configuration.reset` resets the settings of company cards. This method removes the personal settings of the specified user or the general settings defined for all users.

#|
|| **Parameter** | **Description** ||
|| **scope**
[`unknown`](../../../data-types.md) | The scope of the settings. Acceptable values:

- **P** - personal settings,
- **C** - general settings.
 ||
|| **userId**
[`unknown`](../../../data-types.md) | User identifier. If not specified, the current user is used. Required only when resetting personal settings. ||
|#

## Examples

```js
//---
//Reset personal settings of the company card for the user with identifier 1.
BX24.callMethod(
    "crm.company.details.configuration.reset",
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