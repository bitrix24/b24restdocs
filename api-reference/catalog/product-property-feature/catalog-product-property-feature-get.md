# Get the values of the product property feature parameter or trade offers catalog.productPropertyFeature.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- the mandatory parameters are not specified
- there is no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.productPropertyFeature.get(id)
```

Method to access the value of the product property feature or trade offers by ID.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id** 
[`integer`](../../data-types.md)| Identifier of the product property feature or trade offers. ||
|#

{% include [Parameter note](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.productPropertyFeature.get',
    {
        id: 144
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
{% include [Examples note](../../../_includes/examples.md) %}