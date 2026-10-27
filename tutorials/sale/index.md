# Online sales: typical scenarios

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Scenarios help you choose methods for typical online store tasks: adding an item to an order or configuring delivery for the CRM.

A scenario describes a single practical task and the sequence of methods required to perform it.

> Quick links: [all scenarios](#choose-tutorial)
>
> User documentation: [Online store in Bitrix24: Getting started](https://helpdesk.bitrix24.com/open/25809723/)

## Connection with Other Objects

In online sales scenarios, a single order may include line items and delivery. Separate groups of methods are used for each part.

- **Order and basket.** A basket item is an order line containing a product or service. It specifies the quantity, price, and currency. Items are added to an existing order using the [sale.basketitem.add](../../api-reference/sale/basket-item/sale-basket-item-add.md) method. If the item is linked to a product from the catalog, pass the product identifier in the `productId` field. If the product is not in the online store catalog, pass `productId: 0` and fill in the data manually: name, price, quantity, and currency.
- **Product catalog.** If you are adding a product or service to an order, you must fill in the item fields. The [sale.basketitem.getFields](../../api-reference/sale/basket-item/sale-basket-item-get-fields.md) method shows which fields can be passed to the `fields` of the [sale.basketitem.add](../../api-reference/sale/basket-item/sale-basket-item-add.md) method.
- **Delivery services.** A handler links Bitrix24 to an external delivery service. To make the delivery appear in the CRM, register a handler using the [sale.delivery.handler.add](../../api-reference/sale/delivery/handler/sale-delivery-handler-add.md) method, create a delivery service using the [sale.delivery.add](../../api-reference/sale/delivery/delivery/sale-delivery-add.md) method, add shipment properties using the [sale.shipmentproperty.add](../../api-reference/sale/shipment-property/sale-shipment-property-add.md) method, and link the properties to the delivery service using the [sale.propertyRelation.add](../../api-reference/sale/property-relation/sale-property-relation-add.md) method.

If the price is retrieved from the catalog and does not need to be set manually, you can add a product using the [sale.basketitem.addCatalogProduct](../../api-reference/sale/basket-item/sale-basket-item-add-catalog-product.md) and [sale.basketitem.updateCatalogProduct](../../api-reference/sale/basket-item/sale-basket-item-update-catalog-product.md) methods. The minimum set of their fields is returned by the [sale.basketitem.getFieldsCatalogProduct](../../api-reference/sale/basket-item/sale-basket-item-get-catalog-product-fields.md) method.

## What to Consider {#requirements}

Each scenario requires its own scope and permissions. For more information about scopes, see [{#T}](../../api-reference/scopes/permissions.md).

| Scenario | Scope | Who can run it | External service |
|---|---|---|---|
| [Line item in an order](./add-basket-item-to-order.md) | `sale` | Administrator | Not required |
| [Delivery](./delivery-in-crm.md) | `delivery`, `sale` | CRM administrator for `delivery` methods, administrator for `sale` methods | Requires URLs for calculating the cost, creating a delivery request, and canceling it |

- **Authorization.** You can run all scenarios using an inbound webhook or an OAuth token of a user with administrator permissions
- **External service.** A delivery service requires your own server with public HTTPS URLs. Bitrix24 sends requests to these URLs to calculate the delivery cost and to create delivery requests. Without such a server, delivery will not work even if the methods run successfully
- **Secrets.** Do not place tokens, logins, or passwords of external services in public code. Pass them through environment variables or a secure vault

## How to Check the Response and Errors

If the call succeeds, the method returns data in the `result` field. If an error occurs, the response contains the `error` and `error_description` fields instead of `result`. To learn how to recognize an error and where to find its description, see [{#T}](../../error-codes.md).

Errors common to all scenarios:

- `insufficient_scope` — the webhook or application does not have the scope from the table above. Add the scope and repeat the call
- `ACCESS_DENIED` with status `403` — the user does not have sufficient permissions. Check the permissions in the table above

Errors of individual steps are covered in the tutorials:

- [line item in an order](./add-basket-item-to-order.md#errors) — the order was not found, the item currency does not match the order currency
- [delivery](./delivery-in-crm.md#errors) — a handler with this code already exists, there is no handler for the service `REST_CODE`, the property relation already exists

## Getting Started

1. Determine the scenario: line item or delivery.
2. Select a scenario in the [How to choose a scenario](#choose-tutorial) table.
3. Check the scope and permissions in the table in the [What to Consider](#requirements) section.
4. Prepare identifiers for the order, product, payer type, property group, or delivery service. To find where to retrieve object identifiers, see the article [Data types and object structure in the online store REST API](../../api-reference/sale/data-types.md).
5. Execute the methods in the order described in the scenario.
6. Verify the result using `list` or `get` methods: for example, [sale.basketItem.list](../../api-reference/sale/basket-item/sale-basket-item-list.md) for basket items or [sale.delivery.getlist](../../api-reference/sale/delivery/delivery/sale-delivery-get-list.md) for delivery services.

## How to choose a scenario {#choose-tutorial}

#|
|| **If necessary** | **Open** ||
|| Add a product from the catalog or a line item that is not in the catalog to an order and specify an arbitrary price | [How to add a line item to an order with an arbitrary price](./add-basket-item-to-order.md) ||
|| Register an external delivery service to work in CRM | [How to Set Up a Delivery Service for CRM](./delivery-in-crm.md) ||
|| View the online store methods reference | [Internet store: section overview](../../api-reference/sale/index.md) ||
|#
