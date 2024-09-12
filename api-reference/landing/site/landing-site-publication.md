# Publish site landing.site.publication

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.publication` publishes the site (and all its pages).

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **$id**
[`unknown`](../../data-types.md) | Site identifier. | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.site.publication',
    {
        id: 1688
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

{% include [Footnote about examples](../../../_includes/examples.md) %}