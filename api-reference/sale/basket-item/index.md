# Shopping Cart in Online Store: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A cart item is an order line with a product or service: quantity, price, currency, and unit of measurement. The order total is recalculated each time its items change.

The `sale.basketitem.*` methods add, update, retrieve, and delete order cart items.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Place an Order on the Website](https://helpdesk.bitrix24.com/open/17300484/)

## Linking the Cart to Other Objects

**Order.** An item is linked to an order through the `orderId` field — pass the order ID in it. The list of orders can be obtained using the [sale.order.list](../order/sale-order-list.md) method.

**Products.** Pass the product ID in the `productId` field. You can obtain identifiers using the following methods:

- [catalog.product.list](../../catalog/product/catalog-product-list.md) — for simple products
- [catalog.product.service.list](../../catalog/product/service/catalog-product-service-list.md) — for services
- [catalog.product.offer.list](../../catalog/product/offer/catalog-product-offer-list.md) — for variations of products; the parent product with variations is found by the [catalog.product.sku.list](../../catalog/product/sku/catalog-product-sku-list.md) method

**Currency.** Pass the order currency in `currency` — it is returned by the [sale.order.get](../order/sale-order-get.md) method. If the currencies differ, the add method returns an error. The list of Bitrix24 currencies is returned by the [crm.currency.list](../../crm/currency/crm-currency-list.md) method.

**Unit of Measurement.** For a product that is not in the catalog, specify the code and name of the unit of measurement in the `measureCode` and `measureName` fields. Codes and names are returned by the [catalog.measure.list](../../catalog/measure/catalog-measure-list.md) method.

**Payment.** Use the [sale.paymentitembasket.*](../payment-item-basket/index.md) methods to specify which cart items have been paid for.

**Shipment.** Use the [sale.shipmentitem.*](../shipment-item/index.md) methods to specify which cart items to ship.

**Item Properties.** Size, color, and other attributes of a specific item are stored separately and linked to it through `basketId`. Manage them with the [sale.basketproperties.*](../basket-properties/index.md) methods.

## How to Get Started

1. Create an order using [sale.order.add](../order/sale-order-add.md) or find an existing order using [sale.order.list](../order/sale-order-list.md).
2. Add an item using [sale.basketitem.addCatalogProduct](./sale-basket-item-add-catalog-product.md) or [sale.basketitem.add](./sale-basket-item-add.md) — how to choose the method is described [below](#choose-add).
3. Check the cart contents using [sale.basketitem.list](./sale-basket-item-list.md).
4. If necessary, change the quantity or price: for a catalog item, use [sale.basketitem.updateCatalogProduct](./sale-basket-item-update-catalog-product.md); for a custom item, use [sale.basketitem.update](./sale-basket-item-update.md). Then link the item to a payment or shipment.

For an example scenario, see the tutorial [How to Add a Line Item to an Order with an Arbitrary Price](../../../tutorials/sale/add-basket-item-to-order.md).

## How to Choose the Add Method {#choose-add}

**Product or service from the catalog.** Use [sale.basketitem.addCatalogProduct](./sale-basket-item-add-catalog-product.md). The method takes the name, base price, unit of measurement, weight, and VAT from the product card and skips the values passed for these fields.

**Custom item without a product in the catalog.** Use [sale.basketitem.add](./sale-basket-item-add.md) with `productId` = `0` and pass the name, price, and unit of measurement yourself. With a real `productId`, the `add` method also takes data from the catalog, but `addCatalogProduct` is intended for catalog products.

## Response Format

The add, update, and get methods return the item in `result.basketItem`; the [sale.basketitem.delete](./sale-basket-item-delete.md) method returns `true` in `result`. The [sale.basketitem.list](./sale-basket-item-list.md) method returns up to 50 items per call in `result.basketItems` and the total number of matching items in `total`. Request the next page with the `start` parameter. On error, the response contains the `error` and `error_description` fields; the full list of codes is on each method's page. The item fields are described in the [sale_basket_item](../data-types.md#sale_basket_item) type.

## Limitations When Working with Items

- `orderId`, `productId`, and `currency` cannot be changed after the item is added: the update methods skip them without an error. To move an item to another order or replace the product, delete the item and add a new one
- The price passed in `price` is fixed: the item no longer takes its price from the catalog
- [sale.basketitem.list](./sale-basket-item-list.md) also returns items without an order. To retrieve the cart of a single order, filter by `orderId`

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can perform the methods: retrieving items and field descriptions — store manager; adding, updating, and deleting items — administrator

#|
|| **Method** | **Description** ||
|| [sale.basketitem.add](./sale-basket-item-add.md) | Adds a custom item to an order cart ||
|| [sale.basketitem.update](./sale-basket-item-update.md) | Modifies an order cart item ||
|| [sale.basketitem.get](./sale-basket-item-get.md) | Returns a cart item by ID ||
|| [sale.basketitem.list](./sale-basket-item-list.md) | Returns a list of cart items based on a filter ||
|| [sale.basketitem.delete](./sale-basket-item-delete.md) | Removes an item from an order cart ||
|| [sale.basketitem.addCatalogProduct](./sale-basket-item-add-catalog-product.md) | Adds an item with a product or service from the catalog to an order cart ||
|| [sale.basketitem.updateCatalogProduct](./sale-basket-item-update-catalog-product.md) | Modifies a catalog product item ||
|| [sale.basketitem.getFields](./sale-basket-item-get-fields.md) | Returns the description of cart item fields ||
|| [sale.basketitem.getFieldsCatalogProduct](./sale-basket-item-get-catalog-product-fields.md) | Returns the description of catalog product item fields ||
|#