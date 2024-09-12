# Get Product or Offer Image Fields catalog.productImage.getFields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

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
catalog.productImage.getFields()
```

This method returns the fields of the product or offer image.

## Parameters

No parameters.

## Examples

```javascript
BX24.callMethod(
    'catalog.productImage.getFields',
    {},
    function(result) {
        if(result.error())
            console.error(result.error().ex);
        else
            console.log(result.data());
    }
);
```
{% include [Footnote about examples](../../../_includes/examples.md) %}

## Returned Fields

#|
|| **Field** | **Description** | **Note** ||
|| **createTime** 
[`datetime`](../../data-types.md) | Date of addition. | Read-only ||
|| **detailUrl** 
[`string`](../../data-types.md) | Link to the image. | Read-only. ||
|| **downloadUrl** 
[`string`](../../data-types.md) | Link for downloading by the application, signed with the current access_token. | Read-only. ||
|| **id** 
[`integer`](../../data-types.md) | File identifier. | Read-only. ||
|| **name** 
[`string`](../../data-types.md) | File name. | Read-only. ||
|| **productId^*^** 
[`string`](../../data-types.md) | Identifier of the product or offer. | ||
|| **type** 
[`string`](../../data-types.md) | Type of image. Can take three values: 
- `DETAIL_PICTURE` – detailed image;
- `PREVIEW_PICTURE` – preview image;
- `MORE_PHOTO` – image from the "product image" property.  | ||
|#
{% include [Footnote about parameters](../../../_includes/required.md) %}