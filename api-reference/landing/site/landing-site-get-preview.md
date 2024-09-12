# Get URL Preview of the Site landing.site.getPreview

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

The method `landing.site.getPreview` returns the URL of the preview image of the site (preview of the homepage). The site must be accessible for reading.

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **id**
[`unknown`](../../data-types.md) | Site identifier. | ||
|#

## Example

```js
BX24.callMethod(
    'landing.landing.getPreview',
    {
        id: 1817
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