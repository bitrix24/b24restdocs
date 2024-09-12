# Get the types of warehouse accounting documents available for REST catalog.enum.getStoreDocumentTypes

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing response in case of success
- missing response in case of error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.enum.getStoreDocumentTypes()
```

This method returns the types of warehouse accounting documents available for REST.

Currently, the following types are available:
- `A` – Goods arrival at the warehouse;
- `S` – Goods receipt;
- `M` – Transfer of goods between warehouses;
- `R` – Goods return;
- `D` – Goods write-off.

## Parameters

No parameters.

## Examples

```js
BX24.callMethod(
    'catalog.enum.getStoreDocumentTypes',
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