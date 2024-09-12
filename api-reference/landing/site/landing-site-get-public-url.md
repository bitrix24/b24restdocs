# Get Public URL of the site landing.site.getPublicUrl

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.getPublicUrl` returns the full URL of the site(s).

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the site. It can also be an array of identifiers, in which case the response will be an array of site URLs. | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.site.getPublicUrl',
    {
        id: [752, 751]
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

{% include [Footnote on examples](../../../_includes/examples.md) %}