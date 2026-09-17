# Online Store: Overview of Sections

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

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

## How to Get Started

1. Check the structure of fields and types on the page [Data Types and Object Structure in the Online Store REST API](./data-types.md).
2. Verify access permissions and user roles before making changes.
3. Identify the main object of the scenario: order, cart, payment, shipment, or property.
4. Retrieve working identifiers through `list` or `get` in the relevant section.
5. Make changes to the object using the `add`, `update`, or `delete` methods.
6. If necessary, subscribe to [Events](./events/index.md) to track changes in real time.

{% note tip "User Documentation" %}

- [How to Place an Order in the Store](https://helpdesk.bitrix24.com/open/17300484/)
- [How to Create an Order within CRM](https://helpdesk.bitrix24.com/open/8271153/)
- [How to Operate in the Store without Orders](https://helpdesk.bitrix24.com/open/13727858/)

{% endnote %}

## Access Permissions

Access permission settings are not available on all plans. By default, only the Bitrix24 administrator can configure permissions, but they can grant these permissions to other employees.

Access permissions for the Sites and Stores sections are shared. If you change them in one section, the changes will apply to the other.

{% note tip "User Documentation" %}

- [How to Configure Access Permissions for Sites and Online Stores](https://helpdesk.bitrix24.com/open/22057418/)

{% endnote %}

## Relationships with Other Objects

**CRM.** Orders and payments from the online store are used in CRM scenarios where the composition of the order, cost, and statuses are important. To work with orders, use the [Order](./order/index.md) method group, for payments — [Payments](./payment/index.md), and for tracking changes — [Events](./events/index.md).

**Product Catalog.** The online store utilizes product data from the catalog: products, prices, properties, and stock levels. For these scenarios, use the [Product Catalog](../catalog/index.md), [Products](../catalog/product/index.md), and [Price](../catalog/price/index.md) method groups.

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
|| [Order](./order/index.md) | Creating, retrieving, updating, and deleting orders ||
|| [Cart](./basket-item/index.md) | Managing products and services within an order ||
|| [Cart Properties](./basket-properties/index.md) | Managing additional product item data ||
|| [Order Sources](./trade-platform/index.md) | Managing the sources from which orders originate ||
|| [Binding Order Sources to Orders](./trade-binding/index.md) | Linking orders to sources ||
|#

### Order Properties

#| 
|| **Section** | **Description** ||
|| [Order Properties](./property/index.md) | Configuring fields completed in an order ||
|| [Property Groups](./property-group/index.md) | Grouping order properties ||
|| [Order Property Variants of type ENUM](./property-variant/index.md) | Configuring selection options for ENUM properties ||
|| [Property Binding](./property-relation/index.md) | Configuring conditions for displaying order properties ||
|| [Property Values](./property-value/index.md) | Retrieving and updating property values in orders ||
|#

### Payments and Shipments

#| 
|| **Section** | **Description** ||
|| [Payments](./payment/index.md) | Creating and updating order payments ||
|| [Shipments](./shipment/index.md) | Creating and updating order shipments ||
|| [Shipment Item Table](./shipment-item/index.md) | Managing product items in a shipment ||
|| [Shipment Properties](./shipment-property/index.md) | Configuring shipment fields ||
|| [Shipment Property Values](./shipment-property-value/index.md) | Retrieving and updating shipment property values ||
|| [Binding Cart Items to Payments](./payment-item-basket/index.md) | Distributing product items among payments ||
|| [Binding Payments to Shipments](./payment-item-shipment/index.md) | Linking payments to shipments ||
|| [Delivery Services](./delivery/index.md) | Connecting delivery handlers and managing transport requests ||
|| [Cash Registers](./cashbox/index.md) | Configuring cash registers and working with receipts ||
|#

### Statuses and Events

#| 
|| **Section** | **Description** ||
|| [Payer Types](./person-type/index.md) | Configuring customer types for orders ||
|| [Statuses of Payer Types](./business-value-person-domain/index.md) | Matching payer types to individuals and legal entities ||
|| [Statuses](./status/index.md) | Configuring order and delivery statuses ||
|| [Localization of Statuses](./status-lang/index.md) | Configuring status names and descriptions in different languages ||
|| [Events](./events/index.md) | Tracking changes to orders, payments, shipments, and properties ||
|#
