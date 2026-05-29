# Online sales: typical scenarios

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Scenarios help you choose methods for typical online store tasks: adding an item to an order, connecting a cash register, or configuring delivery for the CRM.

A scenario describes a single practical task and the sequence of methods required to perform it.

> Quick links: [all scenarios](#choose-tutorial)
>
> User documentation: [Online store in Bitrix24: Getting started](https://helpdesk.bitrix24.com/open/25809723/)

## Connection with Other Objects

In online sales scenarios, a single order may include line items, receipt fiscalization, and delivery. Separate groups of methods are used for each part.

- **Order and basket.** A basket item is an order line containing a product or service. It specifies the quantity, price, and currency. Items are added to an existing order using the [sale.basketitem.add](../../api-reference/sale/basket-item/sale-basket-item-add.md) method. If the item is linked to a product from the catalog, pass the product identifier in the `productId` field. If the product is not in the online store catalog, pass `productId: 0` and fill in the data manually: name, price, quantity, and currency.
- **Product catalog.** If you are adding a product or service to an order, you must fill in the item fields. The [sale.basketitem.getFields](../../api-reference/sale/basket-item/sale-basket-item-get-fields.md) method shows which fields can be passed to the `fields` of the [sale.basketitem.add](../../api-reference/sale/basket-item/sale-basket-item-add.md) method.
- **Cash registers and receipts.** An external cash register is connected in two steps: first, register a handler using the [sale.cashbox.handler.add](../../api-reference/sale/cashbox/sale-cashbox-handler-add.md) method, then create a cash register using the [sale.cashbox.add](../../api-reference/sale/cashbox/sale-cashbox-add.md) method. The receipt printing result can be retained using the [sale.cashbox.check.apply](../../api-reference/sale/cashbox/sale-cashbox-check-apply.md) method.
- **Delivery services.** A handler links Bitrix24 to an external delivery service. To make the delivery appear in the CRM, register a handler using the [sale.delivery.handler.add](../../api-reference/sale/delivery/handler/sale-delivery-handler-add.md) method, create a delivery service using the [sale.delivery.add](../../api-reference/sale/delivery/delivery/sale-delivery-add.md) method, add shipment properties using the [sale.shipmentproperty.add](../../api-reference/sale/shipment-property/sale-shipment-property-add.md) method, and link the properties to the delivery service using the [sale.propertyRelation.add](../../api-reference/sale/property-relation/sale-property-relation-add.md) method.

If the price is retrieved from the catalog and does not need to be set manually, you can add a product using the [sale.basketitem.addCatalogProduct](../../api-reference/sale/basket-item/sale-basket-item-add-catalog-product.md) and [sale.basketitem.updateCatalogProduct](../../api-reference/sale/basket-item/sale-basket-item-update-catalog-product.md) methods. The minimum set of their fields is returned by the [sale.basketitem.getFieldsCatalogProduct](../../api-reference/sale/basket-item/sale-basket-item-get-catalog-product-fields.md) method.

## Getting Started

1. Determine the scenario: line item, cash register, or delivery.
2. Select a scenario in the [How to choose a scenario](#choose-tutorial) table.
3. Check the permissions and scopes in the selected tutorial.
4. Prepare identifiers for the order, product, payer type, property group, or delivery service. To find where to retrieve object identifiers, see the article [Data types and object structure in the online store REST API](../../api-reference/sale/data-types.md).
5. Execute the methods in the order described in the scenario.
6. Verify the result using `list` or `get` methods: for example, [sale.basketItem.list](../../api-reference/sale/basket-item/sale-basket-item-list.md) for basket items, [sale.cashbox.list](../../api-reference/sale/cashbox/sale-cashbox-list.md) for cash registers, or [sale.delivery.getlist](../../api-reference/sale/delivery/delivery/sale-delivery-get-list.md) for delivery services.

## How to choose a scenario {#choose-tutorial}

#|
|| **If necessary** | **Open** ||
|| Connect an external cash register and transfer receipt printing data | [How to connect a cash register to Bitrix24](./cashbox-add-example.md) ||
|| Add a product from the catalog to an order and specify an arbitrary price | [Create a line item with a product from the catalog in a quantity of 4 units with an arbitrary price](./example-position-with-custom-price.md) ||
|| Add a line item to an order for a product that is not in the online store catalog | [Create a line item with a product that does not exist on the website](./example-position-that-is-not-on-the-site.md) ||
|| Register an external delivery service to work in CRM | [Configure a delivery service for CRM](./delivery-in-crm.md) ||
|| View the online store methods reference | [Internet store: section overview](../../api-reference/sale/index.md) ||
|#
