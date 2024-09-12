# Update site landing.site.update

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

The method `landing.site.update` makes changes to the site. It returns `true` on success or an error.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the entity. | ||
|| **fields**
[`unknown`](../../data-types.md) | Editable fields of the entity. | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.site.update',
    {
        id: 206,
        fields: {
            TITLE: 'My second site!'
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

{% include [Examples note](../../../_includes/examples.md) %}