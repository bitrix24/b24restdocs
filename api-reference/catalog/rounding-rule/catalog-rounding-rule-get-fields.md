# Get Fields of the Price Rounding Rule catalog.roundingRule.getFields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

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
catalog.roundingRule.getFields()
```

This method returns the fields of the price rounding rule.

## Parameters

No parameters.

## Examples

```javascript
BX24.callMethod(
    'catalog.roundingRule.getFields',
    {},
    function(result) {
        if(result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
    }
);
```
{% include [Footnote on examples](../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **catalogGroupId^*^** 
[`integer`](../../data-types.md) | Price type. | ||
|| **createdBy** 
[`integer`](../../data-types.md) | Created by. | Read-only. ||
|| **dateCreate** 
[`datetime`](../../data-types.md) | Creation date. | Read-only. ||
|| **dateModify** 
[`datetime`](../../data-types.md) | Modification date. | Read-only. ||
|| **id** 
[`integer`](../../data-types.md) | Identifier of the price rounding rule. | Read-only. ||
|| **modifiedBy** 
[`integer`](../../data-types.md) | Modified by. | Read-only. ||
|| **price^*^** 
[`double`](../../data-types.md) | Minimum price. | ||
|| **roundPrecision^*^** 
[`double`](../../data-types.md) | Rounding precision. | ||
|| **roundType^*^** 
[`integer`](../../data-types.md) | Rounding type. |  ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}