# Delete page landing.landing.delete

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

The method `landing.landing.delete` removes a page. It returns *true* on success, or an error.

## Parameters

#|
|| **Method** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Entity identifier. ||
|#

## Examples

```js
BX24.callMethod(
    'landing.landing.delete',
    {
        lid: 350
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

{% include [Examples note](../../../../_includes/examples.md) %}