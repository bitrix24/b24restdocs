# Get Information About a Specific Image in a Product or Offer catalog.productImage.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing response in case of error and success
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.productImage.get(productId, id)
```

This method retrieves information about a specific image in a product or offer.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **productId^*^** 
[`string`](../../data-types.md)| The identifier of the product or offer. ||
|| **id^*^** 
[`integer`](../../data-types.md)| The identifier of the image. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.productImage.get',
    {
        productId: 1,
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
{% include [Note on examples](../../../_includes/examples.md) %}