# Delete Image from Product or Offer catalog.productImage.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
catalog.productImage.delete(id)
```

This method deletes an image from a product or offer. If the operation is successful, it returns `true` in the response body.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **productId^*^** 
[`string`](../../data-types.md)| Identifier of the product or offer. ||
|| **id^*^** 
[`integer`](../../data-types.md)| Identifier of the image. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod(
    'catalog.productImage.delete',
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