# Get a List of Product Fields for the Warehouse Document catalog.document.element.getFields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no response in case of an error
- no response in case of success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```js
catalog.document.element.getFields()
```

This method returns a list of product fields for the warehouse document.

## Parameters

No parameters.

## Examples

```js
BX24.callMethod(
    'catalog.document.element.getFields',
    {},
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.log(result.data());
    }
);
```

{% include [Footnote on examples](../../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **amount** 
[`double`](../../../data-types.md) | Quantity. | ||
|| **docId** 
[`integer`](../../../data-types.md) | Document identifier. | Immutable field. ||
|| **elementId** 
[`integer`](../../../data-types.md) | Product identifier [catalog.product.list](../../../catalog/product/catalog-product-list.md). | Immutable field. ||
|| **id** 
[`integer`](../../../data-types.md) | Document product identifier. | Read-only. ||
|| **purchasingPrice** 
[`double`](../../../data-types.md) | Purchase price. | ||
|| **storeFrom** 
[`integer`](../../../data-types.md) | Sender warehouse. | ||
|| **storeTo** 
[`integer`](../../../data-types.md) | Recipient warehouse. | ||
|#

{% include [Footnote on parameters](../../../../_includes/required.md) %}