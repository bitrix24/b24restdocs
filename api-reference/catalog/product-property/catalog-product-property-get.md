# Get Values of Product or Offer Property Fields catalog.productProperty.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- No response in case of an error
- No examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.productProperty.get(id)
```

Method to access the value of product or offer property fields.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id** 
[`integer`](../../data-types.md)| Identifier of the product or offer property. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.productProperty.get',
    {
        id: 128
    },
    function(result)
    {
        if(result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
    }
);
```
{% include [Examples Note](../../../_includes/examples.md) %}