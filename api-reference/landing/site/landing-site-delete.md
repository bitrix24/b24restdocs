# Delete site landing.site.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

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

The method `landing.site.delete` deletes a site. It returns `true` on success, or an error.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **id**
[`unknown`](../../data-types.md) | `id` of the entity. | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.site.delete',
    {
        id: 206
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

{% include [Example notes](../../../_includes/examples.md) %}