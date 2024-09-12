# Get the address of the special page site landing.syspage.getSpecialPage

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- missing parameters or fields
- parameter types not specified
- parameter requirements not specified
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.syspage.getSpecialPage` returns the address of a special page on the site. The example shows how to obtain a link to the personal section page of the site.

```js
BX24.callMethod(
    'landing.syspage.getSpecialPage',
    {
        siteId: 1391,// Site ID
        type: 'personal',// Type of special page
        additional: {// Optional array of additional parameters to be added to the URL
            SECTION: 'private'
        }
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