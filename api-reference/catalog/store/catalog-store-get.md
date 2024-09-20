# Get Warehouse Field Values catalog.store.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- no response in case of an error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.store.get(id)
```

Method to access the values of warehouse fields by ID. If the operation is successful, a [warehouse resource](resource.md) is returned in the response body.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id** 
[`integer`](../../data-types.md)| Warehouse identifier. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.store.get',
    {
        id: 7
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
{% include [Example Notes](../../../_includes/examples.md) %}