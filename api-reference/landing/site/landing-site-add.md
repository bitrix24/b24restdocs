# Add site landing.site.add

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

The method `landing.site.add` adds a site. It returns the `ID` of the created site or an error.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **fields**
[`unknown`](../../data-types.md) | [Entity fields](./base-fields.md) | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.site.add',
    {
        fields: {
            TITLE: 'My first site!',
            CODE: 'firstsite',
            DOMAIN_ID: 'my.bitrix24.site'
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

{% include [Footnote about examples](../../../_includes/examples.md) %}