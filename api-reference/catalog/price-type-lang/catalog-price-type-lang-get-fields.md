# Get Fields for Price Type Translation catalog.priceTypeLang.getFields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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
catalog.priceTypeLang.getFields()
```

This method returns the fields for price type translation.

## Parameters

No parameters.

## Examples

```javascript
BX24.callMethod(
    'catalog.priceTypeLang.getFields',
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

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **catalogGroupId^*^** 
[`integer`](../../data-types.md) | ID of the price type |  ||
|| **id**
[`integer`](../../data-types.md) | Identifier for the price type translation | Immutable ||
|| **lang^*^**
[`string`](../../data-types.md) | Language of the translation |  ||
|| **name^*^**
[`string`](../../data-types.md) | Name of the price type |  ||
|#

{% include [Footnote on parameters](../../../_includes/required.md) %}