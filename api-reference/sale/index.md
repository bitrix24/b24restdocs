# Online Store: Overview of Sections

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The online store in Bitrix24 is a ready-made solution for online sales. It is designed to quickly launch order acceptance, manage the catalog, payment, and delivery within a single system.

The sections of the online store cover the main scenarios:

- creating and modifying orders
- managing the cart and shipment composition
- configuring order properties and their display conditions
- tracking payments, statuses, and cash operations
- integrating with delivery services and handling events

> Quick navigation: [all sections of the online store](#all-methods)
>
> User documentation:
> - [Online Store in Bitrix24: Getting Started](https://helpdesk.bitrix24.com/open/25809723/)
> - [How to Create and Configure an Online Store in Bitrix24](https://helpdesk.bitrix24.com/open/25757867/)
> - [Frequently Asked Questions about the Online Store](https://helpdesk.bitrix24.com/open/25861023/)

## How to Choose a Section

#| 
|| **If you need** | **Open the section** ||
|| To work with orders and the cart | [Order](./order/index.md), [Cart](./basket-item/index.md), [Cart Properties](./basket-properties/index.md) ||
|| To configure properties and field values of orders | [Order Properties](./property/index.md), [Property Groups](./property-group/index.md), [Order Property Variants of type ENUM](./property-variant/index.md), [Property Binding](./property-relation/index.md), [Property Values](./property-value/index.md) ||
|| To work with payments and their relationships | [Payments](./payment/index.md), [Binding Cart Items to Payments](./payment-item-basket/index.md), [Binding Payments to Shipments](./payment-item-shipment/index.md), [Cash Registers](./cashbox/index.md) ||
|| To work with shipments and delivery | [Shipments](./shipment/index.md), [Shipment Item Table](./shipment-item/index.md), [Shipment Properties](./shipment-property/index.md), [Shipment Property Values](./shipment-property-value/index.md), [Delivery Services](./delivery/index.md) ||
|| To configure statuses and payer types | [Statuses](./status/index.md), [Localization of Statuses](./status-lang/index.md), [Payer Types](./person-type/index.md), [Statuses of Payer Types](./business-value-person-domain/index.md) ||
|| To receive events and track changes | [Events](./events/index.md) ||
|#

{% note tip "User Documentation" %}

- [How to Place an Order in the Store](https://helpdesk.bitrix24.com/open/17300484/)
- [How to Create an Order within CRM](https://helpdesk.bitrix24.com/open/8271153/)
- [How to Operate in the Store without Orders](https://helpdesk.bitrix24.com/open/13727858/)

{% endnote %}

## How to Get Started

1. Check the structure of fields and types on the page [Data Types and Object Structure in the Online Store REST API](./data-types.md).
2. Verify access permissions and user roles before making changes.
3. Identify the main object of the scenario: order, cart, payment, shipment, or property.
4. Retrieve working identifiers through `list` or `get` in the relevant section.
5. Make changes to the object using the `add`, `update`, or `delete` methods.
6. If necessary, subscribe to [Events](./events/index.md) to track changes in real-time.

## Access Permissions

Access permission settings are not available on all plans. By default, only the Bitrix24 administrator can configure permissions, but they can grant these permissions to other employees.

Access permissions for the Sites and Stores sections are shared. If you change them in one section, the changes will apply to the other.

{% note tip "User Documentation" %}

- [How to Configure Access Permissions for Sites and Online Stores](https://helpdesk.bitrix24.com/open/22057418/)

{% endnote %}

## Relationships with Other Objects

**CRM.** Orders and payments from the online store are used in CRM scenarios where the composition of the order, cost, and statuses are important. To work with orders, use the [Order](./order/index.md) section, for payments — [Payments](./payment/index.md), and for tracking changes — [Events](./events/index.md).

**Product Catalog.** The online store utilizes product data from the catalog: products, prices, properties, and stock levels. For these scenarios, use the [Product Catalog](../catalog/index.md), [Products](../catalog/product/index.md), and [Price](../catalog/price/index.md) sections.

**Delivery Services.** Delivery is related to shipments, shipment properties, and transport requests in the [Delivery Services](./delivery/index.md) section.

{% note tip "User Documentation" %}

- [CRM + Online Store: Getting Started](https://helpdesk.bitrix24.com/open/13727458/)

{% endnote %}

## Overview of Online Store Sections {#all-methods}

> Scope: [`sale`](../scopes/permissions.md)
>
> Who can perform the method: depending on the method

### Order and Cart

#| 
|| **Section** | **Description** ||
|| [Order](./order/index.md) | Methods for working with orders ||
|| [Cart](./basket-item/index.md) | Methods for working with cart items ||
|| [Cart Properties](./basket-properties/index.md) | Methods for working with cart properties ||
|| [Order Sources](./trade-platform/index.md) | Methods for working with order sources ||
|| [Binding Order Sources to Orders](./trade-binding/index.md) | Methods for working with bindings of order sources ||
|#

### Order Properties

#| 
|| **Section** | **Description** ||
|| [Order Properties](./property/index.md) | Methods for working with order properties ||
|| [Property Groups](./property-group/index.md) | Methods for working with order property groups ||
|| [Order Property Variants of type ENUM](./property-variant/index.md) | Methods for working with ENUM property values of orders ||
|| [Property Binding](./property-relation/index.md) | Methods for working with property bindings of orders ||
|| [Property Values](./property-value/index.md) | Methods for working with order property values ||
|#

### Payments and Shipments

#| 
|| **Section** | **Description** ||
|| [Payments](./payment/index.md) | Methods for working with payments ||
|| [Shipments](./shipment/index.md) | Methods for working with shipments ||
|| [Shipment Item Table](./shipment-item/index.md) | Methods for working with shipment items ||
|| [Shipment Properties](./shipment-property/index.md) | Methods for working with shipment properties ||
|| [Shipment Property Values](./shipment-property-value/index.md) | Methods for working with shipment property values ||
|| [Binding Cart Items to Payments](./payment-item-basket/index.md) | Methods for working with binding cart items to payments ||
|| [Binding Payments to Shipments](./payment-item-shipment/index.md) | Methods for working with binding payments to shipments ||
|| [Delivery Services](./delivery/index.md) | Methods and webhooks for working with delivery services ||
|| [Cash Registers](./cashbox/index.md) | Methods for working with cash registers and receipts ||
|#

### Statuses and Events

#| 
|| **Section** | **Description** ||
|| [Payer Types](./person-type/index.md) | Methods for working with payer types ||
|| [Statuses of Payer Types](./business-value-person-domain/index.md) | Methods for configuring the correspondence of payer types to individuals or legal entities ||
|| [Statuses](./status/index.md) | Methods for working with statuses ||
|| [Localization of Statuses](./status-lang/index.md) | Methods for working with the localization of statuses ||
|| [Events](./events/index.md) | Events for handling changes in the online store ||
|#