# Switch Models landing.role.enable

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- missing parameters or fields
- missing examples
- missing success response
- missing error response

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `landing.role.enable` switches between the extended and role-based models.

## Examples

```js
BX24.callMethod(
    'landing.role.enable',
    {
        mode: 1 // 1 – to enable the role-based model, 0 – to disable (enable the extended model)
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

{% include [Example Note](../../../_includes/examples.md) %}