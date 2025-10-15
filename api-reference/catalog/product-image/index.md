# Product and Variation Images in the Trade Catalog: Overview of Methods

Different types of images can be added to products and variations:
- `DETAIL_PICTURE` — detailed image, the field is available in the old product card,
- `PREVIEW_PICTURE` — preview image, the field is available in the old product card,
- `MORE_PHOTO` — additional images.

## How to Add an Image to a Created Product

To add or replace an image for a created product, use the methods [catalog.productImage.*](#all-methods). Specify the image type and pass an array of two elements in the `fileContent` parameter:
- the name of the image file with its extension,
- the file in base64 encoding.

If the image type is not specified, it will be saved as an additional image `MORE_PHOTO`.

> Quick link: [all methods](#all-methods)

## How to Add an Image When Creating a Product

Use the method [catalog.product.add](../product/catalog-product-add.md) and pass an object in the format `{fileData: [value1, value2]}` to the `previewPicture` or `detailPicture` parameters:
- `value1` — the name of the image file with its extension,
- `value2` — the file in base64 encoding.

{% note tip "Typical use-cases and scenarios" %}

- [How to upload files](../../files/how-to-upload-files.md)

{% endnote %}

## Linking Product and Variation Images with Other Objects

**Products.** Specify the product ID for which you want to add an image. The list of IDs can be obtained using the methods:
- [catalog.product.list](../product/catalog-product-list.md) — for simple products
- [catalog.product.service.list](../product/service/catalog-product-service-list.md) — for services
- [catalog.product.sku.list](../product/sku/catalog-product-sku-list.md) — for parent products of products with variations
- [catalog.product.offer.list](../product/offer/catalog-product-offer-list.md) — for product variations

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can perform the method: administrator

#|
|| **Method** | **Description** ||
|| [catalog.productImage.add](./catalog-product-image-add.md) | Adds an image to a product or variation ||
|| [catalog.productImage.get](./catalog-product-image-get.md) | Returns information about a specific product or variation image ||
|| [catalog.productImage.list](./catalog-product-image-list.md) | Returns a list of product or variation images ||
|| [catalog.productImage.delete](./catalog-product-image-delete.md) | Deletes an image from a product or variation ||
|| [catalog.productImage.getFields](./catalog-product-image-get-fields.md) | Returns available fields for a product or variation image ||
|#