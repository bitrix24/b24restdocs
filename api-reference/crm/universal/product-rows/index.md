# Product Items in CRM Objects: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Product items are lines of goods or services in a CRM card. They store the details of a sale: name, price, quantity, discounts, and taxes. Based on this data, the CRM calculates the total amount for a lead, deal, estimate, invoice, or SPA.

> Quick navigation: [all methods](#all-methods)

## How to Work with Product Items

Each product item is associated with a CRM object: lead, deal, estimate, new invoice, or SPA. To add a product, replace the product list, or retrieve products from a CRM object, specify `ownerType` and `ownerId`.

For example, for a deal with ID `13142`, pass `ownerType: "D"` and `ownerId: 13142`. The code `D` represents a deal; for other codes, refer to the [CRM object types directory](../../data-types.md#object_type). The object identifier can be obtained using the [crm.item.list](../crm-item-list.md) method or from the response of the [crm.item.add](../crm-item-add.md) method.

1. Retrieve the field descriptions of the product item using the [crm.item.productrow.fields](./crm-item-productrow-fields.md) method. This method will help you understand what values can be passed when adding or modifying an item.
2. Add a single product item using the [crm.item.productrow.add](./crm-item-productrow-add.md) method. If you need to replace the entire product list in the CRM object, use the [crm.item.productrow.set](./crm-item-productrow-set.md) method.
3. Retrieve the product items of the CRM object using the [crm.item.productrow.list](./crm-item-productrow-list.md) method. If you need a single item, pass its identifier to the [crm.item.productrow.get](./crm-item-productrow-get.md) method.
4. Modify a product item using the [crm.item.productrow.update](./crm-item-productrow-update.md) method or delete it using the [crm.item.productrow.delete](./crm-item-productrow-delete.md) method if the product is no longer needed in the CRM object.

## Important Considerations

- Access to product items depends on access to the CRM object they belong to. If a user cannot open a deal, invoice, or SPA, they will not be able to retrieve or modify its product items.
- The [crm.item.productrow.*](#all-methods) methods do not work with old invoices. For new integrations with invoices, use the [crm.item.*](../invoice.md) methods.
- The [crm.item.productrow.set](./crm-item-productrow-set.md) method replaces the entire list of product items in the CRM object with the provided set. If you need to add a single item, use [crm.item.productrow.add](./crm-item-productrow-add.md).

## Relationship with Other Objects

**CRM Entities.** Product items are related to a specific CRM object. The relationship is defined through `ownerType` and `ownerId` in the [crm.item.productrow.add](./crm-item-productrow-add.md), [crm.item.productrow.set](./crm-item-productrow-set.md), [crm.item.productrow.list](./crm-item-productrow-list.md), and [crm.item.productrow.getAvailableForPayment](./crm-item-productrow-get-available-for-payment.md) methods.

**Product Catalog.** A product item can be linked to a catalog product through the `productId` field in the [crm.item.productrow.add](./crm-item-productrow-add.md) and [crm.item.productrow.set](./crm-item-productrow-set.md) methods. If you pass `productId` but do not pass `productName` or `measureCode`, the CRM will use the product name and unit of measure from the catalog.

**Payment.** The [crm.item.productrow.getAvailableForPayment](./crm-item-productrow-get-available-for-payment.md) method retrieves product items from the CRM object for which payment has not yet been issued to the client. The CRM object is specified through `ownerType` and `ownerId`.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can perform the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [crm.item.productrow.add](./crm-item-productrow-add.md) | Adds a product item ||
|| [crm.item.productrow.update](./crm-item-productrow-update.md) | Updates a product item ||
|| [crm.item.productrow.get](./crm-item-productrow-get.md) | Retrieves information about a product item ||
|| [crm.item.productrow.list](./crm-item-productrow-list.md) | Retrieves a list of product items ||
|| [crm.item.productrow.delete](./crm-item-productrow-delete.md) | Deletes a product item ||
|| [crm.item.productrow.set](./crm-item-productrow-set.md) | Saves a set of product items in the CRM object ||
|| [crm.item.productrow.getAvailableForPayment](./crm-item-productrow-get-available-for-payment.md) | Retrieves product items without issued payment ||
|| [crm.item.productrow.fields](./crm-item-productrow-fields.md) | Retrieves the field descriptions of product items ||
|#