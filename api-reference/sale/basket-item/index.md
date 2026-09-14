# Shopping Cart in Online Store: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The shopping cart is a temporary storage area where customers add products and services they intend to purchase. In the cart, customers can adjust the quantity of items, remove unwanted products, and view the total cost of their purchase. When the customer completes the purchase, the cart is linked to the order.

This section gathers methods for working with cart items in created orders.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Place an Order on the Website](https://helpdesk.bitrix24.com/open/17300484/)

## Linking the Cart to Other Objects

**Order.** Specify the order to which the cart is linked. The list of orders can be obtained using the [sale.order.list](../order/sale-order-list.md) method.

**Products.** Add products to the cart by specifying their identifiers. You can obtain identifiers using the following methods:
- [catalog.product.list](../../catalog/product/catalog-product-list.md) — for simple products
- [catalog.product.service.list](../../catalog/product/service/catalog-product-service-list.md) — for services
- [catalog.product.sku.list](../../catalog/product/sku/catalog-product-sku-list.md) — for parent products of items with variations
- [catalog.product.offer.list](../../catalog/product/offer/catalog-product-offer-list.md) — for variations of products

**Currency.** Choose the currency in which the price is specified. The list of currencies can be obtained using the [crm.currency.list](../../crm/currency/crm-currency-list.md) method.

**Unit of Measurement.** If you are adding a product to the cart that is not yet on the site, specify the code and name of the unit of measurement. This data can be obtained from the list of units of measurement using the [catalog.measure.list](../../catalog/measure/catalog-measure-list.md) method.

**Linking Cart Item to Payment.** Use the [sale.paymentitembasket.*](../payment-item-basket/index.md) methods to specify which cart items have been paid for.

**Shipment Table Part.** Use the [sale.shipmentitem.*](../shipment-item/index.md) methods to specify which cart items to ship.

## How to Get Started

1. Create an order using [sale.order.add](../order/sale-order-add.md) or find an existing order using [sale.order.list](../order/sale-order-list.md).
2. If you add a product from the catalog, retrieve its ID using catalog methods and use [sale.basketitem.addCatalogProduct](./sale-basket-item-add-catalog-product.md).
3. If you add a custom item, use [sale.basketitem.add](./sale-basket-item-add.md) and pass the product data manually.
4. Check the cart contents using [sale.basketitem.list](./sale-basket-item-list.md).
5. If necessary, change the quantity or price using [sale.basketitem.update](./sale-basket-item-update.md), then link the item to a payment or shipment.

## Overview of Methods {#all-methods}

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can perform the methods: depending on the method

#|
|| **Method** | **Description** ||
|| [sale.basketitem.add](./sale-basket-item-add.md) | Adds an item to the cart of an existing order ||
|| [sale.basketitem.update](./sale-basket-item-update.md) | Modifies a cart item in an existing order ||
|| [sale.basketitem.get](./sale-basket-item-get.md) | Retrieves information about a cart item in an order ||
|| [sale.basketitem.list](./sale-basket-item-list.md) | Returns a set of cart items based on a filter ||
|| [sale.basketitem.delete](./sale-basket-item-delete.md) | Removes a cart item from an order ||
|| [sale.basketitem.getFields](./sale-basket-item-get-fields.md) | Returns available fields of a cart item ||
|| [sale.basketitem.addCatalogProduct](./sale-basket-item-add-catalog-product.md) | Adds an item with a product or service from the catalog module to the cart of an existing order ||
|| [sale.basketitem.updateCatalogProduct](./sale-basket-item-update-catalog-product.md) | Modifies a catalog product in an existing order ||
|| [sale.basketitem.getFieldsCatalogProduct](./sale-basket-item-get-catalog-product-fields.md) | Returns available fields of a catalog product in the cart ||
|#