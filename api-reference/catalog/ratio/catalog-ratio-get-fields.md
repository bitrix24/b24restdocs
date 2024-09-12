# Get Measurement Unit Ratio Fields catalog.ratio.getFields

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing response in case of error
- missing response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
catalog.ratio.getFields()
```

This method returns the fields of the measurement unit ratio.

## Parameters

No parameters.

## Examples

```javascript
BX24.callMethod(
    'catalog.ratio.getFields',
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
{% include [Example Note](../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **id** 
[`integer`](../../data-types.md) | Identifier of the measurement unit. | Read-only. ||
|| **isDefault** 
[`char`](../../data-types.md) | Default measurement unit. |  ||
|| **productId^*^** 
[`integer`](../../data-types.md) | Identifier of the product. | ||
|| **ratio^*^** 
[`double`](../../data-types.md) | Ratio of the measurement unit. | ||
|#
{% include [Parameter Note](../../../_includes/required.md) %}