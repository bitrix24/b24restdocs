# Product and Variation Images in the Trade Catalog: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Different types of images can be added to products and variations:

- `DETAIL_PICTURE` — detailed image, the field is available in the old product card
- `PREVIEW_PICTURE` — preview image, the field is available in the old product card
- `MORE_PHOTO` — additional images

> Quick navigation: [all methods](#all-methods)

## How to Add an Image to a Created Product

To add or replace an image for a created product, use the methods [catalog.productImage.*](#all-methods). Specify the image type and pass an array of two elements in the `fileContent` parameter:

- the name of the image file with its extension
- the file in base64 encoding

If the image type is not specified, it will be saved as an additional image `MORE_PHOTO`.

## How to Add an Image When Creating a Product

Use the method [catalog.product.add](../product/catalog-product-add.md) and pass an object in the format `{fileData: [value1, value2]}` to the `previewPicture` or `detailPicture` parameters:

- `value1` — the name of the image file with its extension
- `value2` — the file in base64 encoding

{% note tip "Typical use-cases and scenarios" %}

- [How to upload files](../../files/how-to-upload-files.md)

{% endnote %}

## Relationship with Other Objects

**Products.** Specify the product ID for which you want to add an image. The list of IDs can be obtained using the methods:

- [catalog.product.list](../product/catalog-product-list.md) — for simple products
- [catalog.product.service.list](../product/service/catalog-product-service-list.md) — for services
- [catalog.product.sku.list](../product/sku/catalog-product-sku-list.md) — for parent products of products with variations
- [catalog.product.offer.list](../product/offer/catalog-product-offer-list.md) — for product variations

## Images in REST 3.0

In [REST 3.0](../../rest-v3.md), images have methods for adding, retrieving, and deleting, as well as the `catalog.product.image.field.list` and `catalog.product.image.field.get` methods for retrieving the field schema. Methods available in both API versions are marked `v 3.0` in the table.

Specifics of the new version methods:

- the [catalog.product.image.get](./catalog-product-image-get-rest-v3.md) and [catalog.product.image.delete](./catalog-product-image-delete-rest-v3.md) methods require the image `id` and the product `productId`, and the [catalog.product.image.list](./catalog-product-image-list-rest-v3.md) method requires a filter by `productId`
- the file is passed in `fields.file.upload`: the file name `name` and the content `data` in Base64, or a `url` link
- images of the `PREVIEW_PICTURE` and `DETAIL_PICTURE` types replace the previous ones, and an image of the `MORE_PHOTO` type is added to the existing ones
- the image identifier matches the file identifier
- in addition to catalog permissions, the methods check the user's permissions for the catalog information block: to read, edit, or delete a product

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can perform the methods:
> - administrator — for methods of the previous API version
> - a user with the "View product catalog", "View catalog products", or "Edit selling price" access permission and permission to read the product — for retrieving images in REST 3.0. Adding and deleting require the "Edit product" access permission and permission to edit the product

#|
|| **Method** | **Description** ||
|| [catalog.productImage.add](./catalog-product-image-add.md) | Adds an image to a product or variation ||
|| [catalog.product.image.add](./catalog-product-image-add-rest-v3.md) | Adds an image to a product or variation v 3.0 ||
|| [catalog.productImage.get](./catalog-product-image-get.md) | Returns information about a specific product or variation image ||
|| [catalog.product.image.get](./catalog-product-image-get-rest-v3.md) | Returns information about a specific product or variation image v 3.0 ||
|| [catalog.productImage.list](./catalog-product-image-list.md) | Returns a list of product or variation images ||
|| [catalog.product.image.list](./catalog-product-image-list-rest-v3.md) | Returns a list of product or variation images v 3.0 ||
|| [catalog.productImage.delete](./catalog-product-image-delete.md) | Deletes an image from a product or variation ||
|| [catalog.product.image.delete](./catalog-product-image-delete-rest-v3.md) | Deletes an image from a product or variation v 3.0 ||
|| [catalog.productImage.getFields](./catalog-product-image-get-fields.md) | Returns available fields for a product or variation image ||
|| [catalog.product.image.field.list](./catalog-product-image-field-list.md) | Returns a list of product image fields v 3.0 ||
|| [catalog.product.image.field.get](./catalog-product-image-field-get.md) | Returns the description of a product image field ||
|#
