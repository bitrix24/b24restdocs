# Get a List of Menu Bindings in `landing.site.getMenuBindings`

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.getMenuBindings` returns a list of bindings associated with the menu (specific or all) Knowledge Bases. Only the bindings for Knowledge Bases that the current user has read access to will be returned.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **menuCode**
[`unknown`](../../../data-types.md) | Symbolic code of the menu, as defined above. Optional; by default, all bindings are returned. | ||
|#

## Example

```js
BX24.callMethod(
    'landing.site.getMenuBindings',
    {
        menuCode: 'crm_switcher:deal'
    },
    function(result)
    {
        if(result.error())
        {
            console.error(result.error());
        }
        else
        {
            console.info(result.data());
        }
    }
);
```

{% include [Footnote on examples](../../../../_includes/examples.md) %}