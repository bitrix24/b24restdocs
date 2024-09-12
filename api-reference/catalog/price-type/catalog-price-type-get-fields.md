# Get Price Type Fields catalog.priceType.getFields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

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
catalog.priceType.getFields()
```

The method returns the fields of the price type.

## Parameters

No parameters.

## Examples

```javascript
BX24.callMethod(
    'catalog.priceType.getFields',
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
|| **base** 
[`string`](../../data-types.md) | Indicates whether the price type is base. You can set the value `Y` for any price type, and that type will become the base. | ||
|| **createdBy** 
[`integer`](../../data-types.md) | Who created it. | Read-only. ||
|| **dateCreate** 
[`datetime`](../../data-types.md) | Creation date. | Read-only. ||
|| **id** 
[`integer`](../../data-types.md) | Identifier. | Read-only. ||
|| **modifiedBy** 
[`integer`](../../data-types.md) | Who modified it. | Read-only. ||
|| **name^*^** 
[`string`](../../data-types.md) | Code of the price type. | ||
|| **sort** 
[`integer`](../../data-types.md) | Sorting order. | ||
|| **timestampX** 
[`datetime`](../../data-types.md) | Modification date. | Read-only. ||
|| **xmlId** 
[`string`](../../data-types.md) | External code. | ||
|#
{% include [Footnote about parameters](../../../_includes/required.md) %}