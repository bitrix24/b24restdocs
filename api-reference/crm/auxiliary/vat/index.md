# VAT Rates: Overview of Methods

Setting the VAT rate for products or product items allows for automation of:

-  calculation of the final cost of a product or service
-  data transfer to documents
-  integration with payment systems
-  data exchange with cash registers

> Quick navigation: [all methods](#all-methods) 
>
> User documentation: [Taxes in CRM](https://helpdesk.bitrix24.com/open/17230492/)

## Connection of VAT Rates with Other Objects

**Product items** are goods or services listed in a deal or another CRM entity. Product items may not be present in the trade catalog. Changing the VAT rate for a product in one deal does not affect the rate of a similar product in another entity. To set VAT for a product item in a deal or another CRM entity, use the `taxRate` parameter of the [crm.item.productrow.*](../../universal/product-rows/index.md) method group.

**Goods and services** are company products with fixed sales conditions stored in the trade catalog. When selecting a product from the catalog in a deal or another CRM entity, it will automatically be added with the rate specified in the catalog. To set the VAT for a product or service in the trade catalog, use the `vatId` parameter of the [catalog.product.*](../../../catalog/product/index.md) method group.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#|
|| **Method** | **Description** ||
|| [crm.vat.add](./crm-vat-add.md) | Creates a new VAT rate ||
|| [crm.vat.delete](./crm-vat-delete.md) | Deletes a VAT rate ||
|| [crm.vat.get](./crm-vat-get.md) | Returns the VAT rate by identifier ||
|| [crm.vat.fields](./crm-vat-fields.md) | Returns the description of VAT rate fields ||
|| [crm.vat.list](./crm-vat-list.md) | Returns a list of VAT rates ||
|| [crm.vat.update](./crm-vat-update.md) | Updates an existing VAT rate ||
|#