# Get a List of Rounding Types catalog.enum.getRoundTypes

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing response on success
- missing response on error
- no examples in other languages

{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.enum.getRoundTypes()
```

The method returns a list of rounding types.

## Parameters

No parameters.

## Examples

```js
BX24.callMethod(
    'catalog.enum.getRoundTypes',
    {},
    function(result)
    {
        if(result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
    }
);
```
{% include [Footnote on examples](../../../_includes/examples.md) %}