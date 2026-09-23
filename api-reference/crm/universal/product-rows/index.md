# Product Items in CRM Objects: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Product items are lines of goods or services in a CRM card. They store the details of a sale: name, price, quantity, discounts, and taxes. Based on this data, the CRM calculates the total amount for a lead, deal, estimate, invoice, or SPA. The full composition of a product item is described in the [crm_item_product_row](../../data-types.md#crm_item_product_row) object.

> Quick navigation: [all methods](#all-methods)

## Linking a Product Item to a CRM Object

Every product item belongs to a CRM object. To add, replace, or retrieve product items, specify `ownerType` and `ownerId`. For example, for a deal with ID `13142`, pass `ownerType: "D"` and `ownerId: 13142`. The object identifier can be retrieved with the [crm.item.list](../crm-item-list.md) method or taken from the response of the [crm.item.add](../crm-item-add.md) method.

Not every CRM object has product items. The methods of this section work with the following types:

#|
|| **CRM Object** | **`ownerType` Code** ||
|| Lead | `L` ||
|| Deal | `D` ||
|| Estimate | `Q` ||
|| Invoice (new) | `SI` ||
|| SPA | `T` followed by the hexadecimal type identifier. For example, for a type with `entityTypeId: 128` the code is `T80` ||
|#

The complete list of codes is in the [CRM object types](../../data-types.md#object_type) directory.

The methods of this section do not serve the other types of CRM objects, but they behave differently on them:

- contacts and companies have no product items. The [crm.item.productrow.list](./crm-item-productrow-list.md) and [crm.item.productrow.getAvailableForPayment](./crm-item-productrow-get-available-for-payment.md) methods return an empty result for them, and write methods return an error
- old invoices with the `I` code are not supported at all. Write methods and [crm.item.productrow.getAvailableForPayment](./crm-item-productrow-get-available-for-payment.md) return the `ENTITY_TYPE_NOT_SUPPORTED` error, and [crm.item.productrow.list](./crm-item-productrow-list.md) returns the `ACCESS_DENIED` error

For new integrations with invoices, use the methods of the [Invoices](../invoice.md) section.

## Workflow with Product Items

1. Retrieve the field descriptions of the product item using the [crm.item.productrow.fields](./crm-item-productrow-fields.md) method. This method will help you understand what values can be passed when adding or modifying an item.
2. Add a single product item using the [crm.item.productrow.add](./crm-item-productrow-add.md) method, or save the whole set at once using the [crm.item.productrow.set](./crm-item-productrow-set.md) method.
3. Retrieve the product items of the CRM object using the [crm.item.productrow.list](./crm-item-productrow-list.md) method. If you need a single item, pass its identifier to the [crm.item.productrow.get](./crm-item-productrow-get.md) method.
4. Modify a product item using the [crm.item.productrow.update](./crm-item-productrow-update.md) method or delete it using the [crm.item.productrow.delete](./crm-item-productrow-delete.md) method if the item is no longer needed in the CRM object.

## Limitations and Specifics

- The [crm.item.productrow.*](#all-methods) methods are the current way to work with product items. Development of the `crm.deal.productrows.*`, `crm.lead.productrows.*`, and `crm.quote.productrows.*` methods has been stopped. Use the methods of this section in new integrations.
- Access to product items depends on access to the CRM object they belong to. If a user cannot open a deal, invoice, or SPA, they will neither retrieve nor modify its product items. The [crm.item.productrow.fields](./crm-item-productrow-fields.md) method is the exception: it only describes the fields and requires no permissions for CRM objects.
- A product item has no currency field of its own. Prices are retained in the currency of the CRM object the item belongs to.
- The length of the text fields of a product item is limited. The maximum values are listed in the [{#T}](../../field-length-limits.md) article.

## Relationship with Other Objects

Product items are related to CRM objects, the product catalog, and payments.

**CRM Objects.** A product item does not exist on its own — it always belongs to a lead, deal, estimate, invoice, or SPA. This relationship is defined by the `ownerType` and `ownerId` pair of fields, which is accepted by the [crm.item.productrow.add](./crm-item-productrow-add.md), [crm.item.productrow.set](./crm-item-productrow-set.md), [crm.item.productrow.list](./crm-item-productrow-list.md), and [crm.item.productrow.getAvailableForPayment](./crm-item-productrow-get-available-for-payment.md) methods.

**Product Catalog.** An item can refer to a product of the trade catalog — in that case, the name and the unit of measure are taken from the catalog. This relationship is defined by the `productId` field in the [crm.item.productrow.add](./crm-item-productrow-add.md), [crm.item.productrow.update](./crm-item-productrow-update.md), and [crm.item.productrow.set](./crm-item-productrow-set.md) methods. The catalog products themselves are retrieved by the methods of the [Trade Catalog](../../../catalog/index.md) section.

**Payments.** A payment is issued to the customer based on the product items of a CRM object. The [crm.item.productrow.getAvailableForPayment](./crm-item-productrow-get-available-for-payment.md) method selects the items that have not been included in payments yet, and they are further processed by the methods of the [Payments and Deliveries](../payment/index.md) and [Product Items in Payment](../payment/products-in-payment/index.md) sections.

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

## Continue Learning

- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-product-binding.md)
- [{#T}](../payment/index.md)
- [{#T}](../payment/products-in-payment/index.md)
- [{#T}](../invoice.md)
- [{#T}](../index.md)
