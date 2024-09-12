# Get Group Bindings landing.site.getGroupBindings

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.getGroupBindings` allows you to find out if there is a binding to a group, or what bindings to groups exist. Only bindings to Knowledge Bases that the current user has read access to will be returned.

## Parameters

#|
|| **Parameter** | **Description** | **Since version** ||
|| **groupId**
[`unknown`](../../../data-types.md) | The identifier of the group for which to return the binding. Optional; by default, all bindings to any groups are returned. | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.site.getGroupBindings',
    {
        groupId: 174
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