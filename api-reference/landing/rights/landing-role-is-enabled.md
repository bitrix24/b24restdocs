# Define the models of access permissions landing.role.isEnabled

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `landing.role.isEnabled` determines which model is currently enabled in the project, role-based or extended.

## Parameters

No parameters.

## Examples

```js
BX24.callMethod(
    'landing.role.isEnabled',
    {
    },
    function(result)
    {
        if(result.error())
        {
            console.error(result.error());
        }
        else
        {
            if (result.data())
            {
                console.log('Role-based model');
            }
            else
            {
                console.log('Extended model');
            }
        }
    }
);
```

{% include [Footnote on examples](../../../_includes/examples.md) %}