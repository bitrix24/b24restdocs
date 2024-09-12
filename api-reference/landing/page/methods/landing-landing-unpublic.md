# Unpublish Page landing.landing.unpublic

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

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.landing.unpublic` unpublishes pages. It returns *true* or an error.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier. ||
|#

## Examples

```js
BX24.callMethod(
    'landing.landing.unpublic',
    {
        lid: 351
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

{% include [Example Notes](../../../../_includes/examples.md) %}