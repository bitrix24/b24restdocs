# Get the list of fields for inventory document items catalog.document.element.getFields

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

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can subscribe: any user

## Description

```js
catalog.document.element.getFields()
```

The method returns a list of fields for inventory document items.

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

{% include [Footnote about examples](../../../../_includes/examples.md) %}

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
[`integer`](../../../data-types.md) | Document item identifier. | Read-only. ||
|| **purchasingPrice** 
[`double`](../../../data-types.md) | Purchase price. | ||
|| **storeFrom** 
[`integer`](../../../data-types.md) | Sender inventory. | ||
|| **storeTo** 
[`integer`](../../../data-types.md) | Recipient inventory. | ||
|#

{% include [Footnote about parameters](../../../../_includes/required.md) %}