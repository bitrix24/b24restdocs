# Get Warehouse Fields catalog.store.getFields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

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
catalog.store.getFields()
```

The method returns the fields of the warehouses.

## Parameters

No parameters.

## Examples

```js
BX24.callMethod(
    'catalog.store.getFields',
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

{% include [Examples note](../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **active** 
[`char`](../../data-types.md) | Activity. | ||
|| **address^*^** 
[`string`](../../data-types.md) | Address. |  ||
|| **code** 
[`string`](../../data-types.md) | Symbolic code. |  ||
|| **dateCreate** 
[`datetime`](../../data-types.md) | Creation date. |  ||
|| **dateModify** 
[`datetime`](../../data-types.md) | Modification date. |  ||
|| **description** 
[`string`](../../data-types.md) | Description. |  ||
|| **email** 
[`string`](../../data-types.md) | E-mail. |  ||
|| **gpsN** 
[`double`](../../data-types.md) | GPS latitude. |  ||
|| **gpsS** 
[`double`](../../data-types.md) | GPS longitude. | ||
|| **id** 
[`integer`](../../data-types.md) | Warehouse identifier. | Read-only. ||
|| **imageId** 
[`integer`](../../data-types.md) | Image identifier. |  ||
|| **issuingCenter** 
[`char`](../../data-types.md) | Pickup point. |  ||
|| **locationId** 
[`integer`](../../data-types.md) | Location identifier. |  ||
|| **modifiedBy** 
[`integer`](../../data-types.md) | Modified by. |  ||
|| **phone** 
[`string`](../../data-types.md) | Phone. |  ||
|| **schedule** 
[`string`](../../data-types.md) | Working hours. |  ||
|| **shippingCenter** 
[`char`](../../data-types.md) | For shipping. |  ||
|| **siteId** 
[`string`](../../data-types.md) | Site identifier. |  ||
|| **sort** 
[`integer`](../../data-types.md) | Sorting. |  ||
|| **title** 
[`string`](../../data-types.md) | Title. |  ||
|| **userId** 
[`integer`](../../data-types.md) | Created by. |  ||
|| **xmlId** 
[`string`](../../data-types.md) | External code. |  ||
|#

{% include [Parameters note](../../../_includes/required.md) %}