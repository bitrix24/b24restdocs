# Get Price Field Values catalog.price.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.price.get(id)
```

Method to access the price field values by ID. If the operation is successful, a [price resource](resource.md) is returned in the response body.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id** 
[`integer`](../../data-types.md)| Identifier of the product price. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.price.get',
    {
        id: 122
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