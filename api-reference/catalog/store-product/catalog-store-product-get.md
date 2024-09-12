# Get Inventory Field Values by Warehouse catalog.storeproduct.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the requiredness of parameters is not specified
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
catalog.storeproduct.get(id)
```

Method to access the value of inventory fields by warehouse.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **id** 
[`integer`](../../data-types.md)| Primary key of the record. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.storeproduct.get',
    {
        id: 1
    },
    function(result) {
        if(result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
    }
);
```
{% include [Examples Note](../../../_includes/examples.md) %}