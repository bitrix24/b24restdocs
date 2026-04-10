# VAT Rates: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "" %}

**DEPRECATED**

The development of methods crm.vat.* has been halted.  
Please use the section [Universal methods for invoices](../../universal/invoice.md).

{% endnote %}

Configuring the VAT rate for products or product items allows for the automation of:

-  calculating the final cost of a product or service
-  transferring data to documents
-  integrating with payment systems
-  exchanging data with cash registers

> Quick navigation: [all methods](#all-methods)  
> User documentation: [Taxes in CRM](https://helpdesk.bitrix24.com/open/17230492/)

{% note warning "Development of methods has been halted" %}

The methods `crm.vat.*` continue to function, but there is a more relevant alternative: [catalog.vat.*](../../../catalog/vat/index.md).

{% endnote %}

## Connection of VAT Rates with Other Objects

**Product items** are goods or services listed in a deal or another CRM entity. Product items may not be present in the trade catalog. Changing the VAT rate for a product in one deal does not affect the rate of a similar product in another entity. To set the VAT for a product item in a deal or another CRM entity, use the `taxRate` parameter from the group of methods [crm.item.productrow.*](../../universal/product-rows/index.md).

**Goods and services** are products of the company with fixed sales conditions, stored in the trade catalog. When selecting a product from the catalog in a deal or another CRM entity, it will automatically be added with the rate specified in the catalog. To set the VAT for a product or service in the trade catalog, use the `vatId` parameter from the group of methods [catalog.product.*](../../../catalog/product/index.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)  
> Who can execute the methods: depending on the method

#|  
|| **Method** | **Description** ||  
|| [crm.vat.add](./crm-vat-add.md) | Creates a new VAT rate ||  
|| [crm.vat.delete](./crm-vat-delete.md) | Deletes a VAT rate ||  
|| [crm.vat.get](./crm-vat-get.md) | Returns a VAT rate by ID ||  
|| [crm.vat.fields](./crm-vat-fields.md) | Returns the description of VAT rate fields ||  
|| [crm.vat.list](./crm-vat-list.md) | Returns a list of VAT rates ||  
|| [crm.vat.update](./crm-vat-update.md) | Updates an existing VAT rate ||  
|#