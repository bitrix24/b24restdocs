# Add Product Price catalog.price.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the requiredness of parameters is not specified
- no response in case of success
- no response in case of error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.price.add(fields)
```

This method adds a product price.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields**
[`object`](../../data-types.md)| Fields corresponding to the available list of fields [`fields`](catalog-price-get-fields.md). ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.price.add',
    {
        fields: {
            catalogGroupId: 1,
            currency: "USD",
            price: 2000,
            productId: 1
        }
    },
    function(result) {
        if (result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
    }
);
```
{% include [Note on examples](../../../_includes/examples.md) %}