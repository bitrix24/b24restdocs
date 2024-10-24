# Delete Price Type Binding from Customer Group catalog.priceTypeGroup.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.priceTypeGroup.delete(id)
```

This method removes the product price from the collection of product prices. If the operation is successful, it returns `true` in the response body.

## Parameters

#| 
|| **Parameter** | **Description** ||
|| **id** 
[`integer`](../../data-types.md)| Identifier of the price type binding to the customer group. ||
|#

{% include [Note on parameters](../../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.priceTypeGroup.delete',
    {
        id: 84
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
```
{% include [Note on examples](../../../../_includes/examples.md) %}