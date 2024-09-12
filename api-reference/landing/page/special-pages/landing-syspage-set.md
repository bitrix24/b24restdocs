# Set a Special Page for the Site landing.syspage.set

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing parameters or fields
- parameter types not specified
- required parameters not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.syspage.set` sets a special page for the site. If the **lid** parameter is not provided, the corresponding page type, not the page itself, will be removed. The method does not return a result.

## Examples

```js
BX24.callMethod(
    'landing.syspage.set',
    {
        id: 1390,// Site ID
        type: 'personal',// Page type
        lid: 8593// ID of the page that will be considered of this type within the site
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
)
```

{% include [Footnote on examples](../../../../_includes/examples.md) %}