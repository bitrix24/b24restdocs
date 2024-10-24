# Delete Translation of Price Type catalog.priceTypeLang.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

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
catalog.priceTypeLang.delete(id)
```

This method deletes the translation of the price type name. If the operation is successful, it returns `true` in the response body.

## Parameters

#| 
|| **Parameter** | **Description** ||
|| **id** 
[`integer`](../../data-types.md)| Identifier of the price type name translation. ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.priceTypeLang.delete',
    {
        id: 346
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