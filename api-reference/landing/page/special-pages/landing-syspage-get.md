# Get a list of special pages landing.syspage.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing parameters or fields
- parameter types not specified
- required parameters not indicated
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.syspage.get` returns a list of website pages that are set as special.

## Examples

```js
BX24.callMethod(
    'landing.syspage.get',
    {
        id: 1390,// Site ID
        active: true// If true, only active pages of the site will be returned (default is all)
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

{% include [Note on examples](../../../../_includes/examples.md) %}