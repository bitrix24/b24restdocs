# Get Values of the Translation Fields for Price Type catalog.priceTypeLang.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- required parameters are not specified
- no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.priceTypeLang.get(id)
```

This method provides access to the value of the translation fields for the price type name.

## Parameters

#| 
|| **Parameter** | **Description** ||
|| **id** 
[`integer`](../../data-types.md)| Identifier for the translation of the price type name. ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.priceTypeLang.get',
    {
        id: 537
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
{% include [Example Notes](../../../../_includes/examples.md) %}