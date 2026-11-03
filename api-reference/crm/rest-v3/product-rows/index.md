# Product Items in REST 3.0: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Product items are rows with products and services in a CRM item form: name, price, quantity, discount, and tax. CRM calculates the item amount from the product items unless it was set manually. The `crm.<type>.productrow.*` methods add, update, replace, and delete the product items of one item and return the unpaid remainder for issuing a payment.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Add Products to Lead, Deal, Estimate, and Invoice Forms](https://helpdesk.bitrix24.com/open/14303190/)

## Object Types with Product Items

The object type is set by the `<type>` segment of the method name: `crm.deal.productrow.add` works with deal product items, and `crm.smartprocess1038.productrow.add` works with the product items of the SPA with `entityTypeId = 1038`.

#|
|| **Object type** | **Segment in the method name** ||
|| Deal | `deal` ||
|| Lead | `lead` ||
|| Estimate | `quote` ||
|| Invoice | `smartinvoice` ||
|| CRM document | `smartdocument` ||
|| SPA | `smartprocess<entityTypeId>`, for example, `smartprocess1038` ||
|#

Contacts, companies, and my companies have no product items. An SPA has the methods only if linking to catalog products is enabled in the type settings — without it, a call returns the "Method Not Found" error. SPA type IDs are returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method in the `entityTypeId` field.

Method names are written in lowercase: `crm.deal.productrow.getavailableforpayment`.

## Relationship with Other Objects

A product item is linked to a CRM item and a catalog product.

**CRM item.** A product item belongs to one item — its ID is stored in the `ownerId` field. The item is set when the product item is added and does not change afterwards. The `list`, `replace`, and `getavailableforpayment` methods work with the product items of one item: `ownerId` is required. Product items cannot be passed in `fields` of the [crm.&lt;type&gt;.add](../items/crm-entity-add-rest-v3.md) method — create the item first, then add product items. Permission to view an item grants access to read its product items, and permission to edit it grants access to change them.

**Product catalog.** The `productId` field refers to a catalog product. If `productName` is not passed when adding, the name is taken from the catalog. A product item can also be created without a catalog product — then `productId` equals `0`, and the name is passed in `productName`. Products are returned by the [catalog.product.list](../../../catalog/product/catalog-product-list-rest-v3.md) method, and units of measure by [catalog.measure.list](../../../catalog/measure/catalog-measure-list-rest-v3.md).

**Payments.** The [crm.&lt;type&gt;.productrow.getavailableforpayment](./crm-entity-product-row-get-available-for-payment-rest-v3.md) method returns product items with a remainder that is not yet included in payments. Payments are created with the [crm.item.payment.*](../../universal/payment/index.md) methods.

## Product Item Fields {#fields}

Field names are written in camelCase. Fields are returned only if they are listed in `select`, otherwise a product item contains only `id`.

#|
|| **Field**
`type` | **Description** | **Editable** ||
|| **id**
[`integer`](../../../data-types.md) | Product item ID | No ||
|| **ownerId**
[`integer`](../../../data-types.md) | ID of the CRM item the product item belongs to | Only when adding ||
|| **ownerTypeId**
[`integer`](../../../data-types.md) | Object type ID of the item, for example, `2` — deal, `1038` — the SPA with this `entityTypeId` | No ||
|| **productId**
[`integer`](../../../data-types.md) | Catalog product ID. `0` — a product item without a catalog product | Yes ||
|| **productName**
[`string`](../../../data-types.md) | Product item name | Yes ||
|| **price**
[`double`](../../../data-types.md) | Unit price including discount and tax | Yes ||
|| **quantity**
[`double`](../../../data-types.md) | Quantity | Yes ||
|| **discountTypeId**
[`integer`](../../../data-types.md) | Discount type: `1` — fixed amount, `2` — percentage | Yes ||
|| **discountRate**
[`double`](../../../data-types.md) | Discount percentage | Yes ||
|| **discountSum**
[`double`](../../../data-types.md) | Discount amount per unit | Yes ||
|| **taxRate**
[`double`](../../../data-types.md) | Tax rate in percent. `null` — a product item without tax | Yes ||
|| **taxIncluded**
[`boolean`](../../../data-types.md) | `true` — the tax is included in the price, `false` — the tax is added on top of the price | Yes ||
|| **measureCode**
[`integer`](../../../data-types.md) | Unit of measure code, for example, `796` — piece | Yes ||
|| **measureName**
[`string`](../../../data-types.md) | Unit of measure name | Yes ||
|| **sort**
[`integer`](../../../data-types.md) | Position of the product item in the list | Yes ||
|| **productTypeId**
[`integer`](../../../data-types.md) | Catalog product type: `1` — simple product, `2` — bundle, `3` — product with variations, `4` — variation, `5` — variation without a product, `6` — product with variations but without variations, `7` — service | No ||
|#

You can filter the list by `ownerId`, `id`, and `productId`, and sort it by `sort` and `id`.

The description of fields with the `editable`, `filterable`, and `sortable` flags is returned by the `crm.<type>.productrow.field.list` and `crm.<type>.productrow.field.get` methods. They work like the other REST 3.0 field methods: `field.list` takes `select` with a list of flags, and `field.get` also takes `name` with the field name, for example, `{"name":"price"}`.

## Add or Replace Product Items

There are two ways to change the product items of an item:

- one at a time: [crm.&lt;type&gt;.productrow.add](./crm-entity-product-row-add-rest-v3.md), [crm.&lt;type&gt;.productrow.update](./crm-entity-product-row-update-rest-v3.md), and [crm.&lt;type&gt;.productrow.delete](./crm-entity-product-row-delete-rest-v3.md). The IDs of the other product items do not change
- the whole set: [crm.&lt;type&gt;.productrow.replace](./crm-entity-product-row-replace-rest-v3.md) writes the passed set instead of the current one. Product items that are not in the set are deleted, and changed product items get new IDs. An empty set deletes all product items

## How to Get Started

1. Determine the object type and retrieve the item ID with the [crm.&lt;type&gt;.list](../items/crm-entity-list-rest-v3.md) method, or create an item with the [crm.&lt;type&gt;.add](../items/crm-entity-add-rest-v3.md) method
2. Find products in the catalog with the [catalog.product.list](../../../catalog/product/catalog-product-list-rest-v3.md) method. Skip this step for a product item without a catalog product
3. Add product items with the [crm.&lt;type&gt;.productrow.add](./crm-entity-product-row-add-rest-v3.md) method, or set the entire set in one call with [crm.&lt;type&gt;.productrow.replace](./crm-entity-product-row-replace-rest-v3.md)
4. Check the product items with the [crm.&lt;type&gt;.productrow.list](./crm-entity-product-row-list-rest-v3.md) method and the item amount with the [crm.&lt;type&gt;.get](../items/crm-entity-get-rest-v3.md) method with the `opportunity` field in `select`

## Methods of the Previous Version

At the `/rest/` address, product items are handled by the [crm.item.productrow.*](../../universal/product-rows/index.md) methods. They use uppercase fields and the object type in the `ownerType` parameter, for example, `D` for a deal. REST 3.0 methods set the object type in the method name, and fields are written in camelCase.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#|
|| **Method** | **Description** ||
|| [crm.&lt;type&gt;.productrow.add](./crm-entity-product-row-add-rest-v3.md) | Adds a product item to an item ||
|| [crm.&lt;type&gt;.productrow.update](./crm-entity-product-row-update-rest-v3.md) | Updates a product item ||
|| [crm.&lt;type&gt;.productrow.get](./crm-entity-product-row-get-rest-v3.md) | Returns a product item by ID ||
|| [crm.&lt;type&gt;.productrow.list](./crm-entity-product-row-list-rest-v3.md) | Returns the product items of an item ||
|| [crm.&lt;type&gt;.productrow.delete](./crm-entity-product-row-delete-rest-v3.md) | Deletes a product item ||
|| [crm.&lt;type&gt;.productrow.replace](./crm-entity-product-row-replace-rest-v3.md) | Replaces all product items of an item ||
|| [crm.&lt;type&gt;.productrow.getavailableforpayment](./crm-entity-product-row-get-available-for-payment-rest-v3.md) | Returns product items that are not yet included in payments ||
|| `crm.<type>.productrow.field.list` | Returns the description of product item fields ||
|| `crm.<type>.productrow.field.get` | Returns the description of a product item field by name ||
|#
