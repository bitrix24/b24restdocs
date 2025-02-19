# VAT Rates in the Trade Catalog: Overview of Methods

The VAT rate can be set for the entire catalog or for a specific product. The product's VAT rate takes precedence. The catalog rate is applied if the product rate is not specified.

If a product or catalog has a VAT rate, general CRM taxes do not apply to them.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: [Taxes in CRM](https://helpdesk.bitrix24.com/open/17230492/)

## Relationship of VAT Rates with Other Objects

**Trade Catalog.** Find out what VAT rate is set for the trade catalog using the method [catalog.catalog.get](../catalog/catalog-catalog-get.md).

**Products.** Specify the product's VAT using the following groups of methods:
- [catalog.product.*](../product/index.md) — for simple products
- [catalog.product.service.*](../product/service/index.md) — for services
- [catalog.product.sku.*](../product/sku/index.md) — for parent products of products with variations
- [catalog.product.offer.*](../product/offer/index.md) — for variations of products

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

#| 
|| **Method** | **Description** ||
|| [catalog.vat.add](./catalog-vat-add.md) | Adds a VAT rate ||
|| [catalog.vat.update](./catalog-vat-update.md) | Modifies a VAT rate ||
|| [catalog.vat.get](./catalog-vat-get.md) | Returns the VAT rate field values by identifier ||
|| [catalog.vat.list](./catalog-vat-list.md) | Returns a list of VAT rates by filter ||
|| [catalog.vat.delete](./catalog-vat-delete.md) | Deletes a VAT rate ||
|| [catalog.vat.getFields](./catalog-vat-get-fields.md) | Returns the fields of the VAT rate ||
|#