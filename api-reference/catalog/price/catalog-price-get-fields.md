# Get Price Fields catalog.price.getFields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no response in case of error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
catalog.price.getFields()
```

The method returns the price fields of a product.

## Parameters

No parameters.

## Examples

```javascript
BX24.callMethod(
    'catalog.price.getFields',
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
{% include [Footnote about examples](../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **catalogGroupId^*^** 
[`integer`](../../data-types.md) | Price type | Immutable ||
|| **currency^*^** 
[`string`](../../data-types.md) | Currency |  ||
|| **extraId**
[`integer`](../../data-types.md) | Surcharge identifier | ||
|| **id**
[`integer`](../../data-types.md) | Price identifier | Read-only. ||
|| **price^*^**
[`double`](../../data-types.md) | Price |  ||
|| **priceScale** 
[`double`](../../data-types.md) | Base price |  ||
|| **productId^*^**
[`integer`](../../data-types.md) | Product identifier | Immutable ||
|| **quantityFrom** 
[`integer`](../../data-types.md) | Quantity from | ||
|| **quantityTo** 
[`integer`](../../data-types.md) | Quantity to | ||
|| **timestampX** 
[`datetime`](../../data-types.md) | Date of change | Read-only. ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}