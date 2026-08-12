# Linking Orders to CRM Entities: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Online store orders can be linked to CRM entities. This allows you to use order information in workflows related to deals and invoices.

A link is a separate record, not a field of the order or of the CRM object. An order can have only one link: if you create a link for an order that already has one, the previous link is replaced by the new one.

> Quick Navigation: [all methods](#all-methods)

## Linking to Other Entities

**Online Store Orders.** The order identifier `orderId` links the order record to a CRM object. The orders themselves are handled by the [sale.order.*](../../../sale/order/index.md) methods.

**CRM Entities.** The linking is supported for deals and invoices using the parameter pair `ownerTypeId` and `ownerId`. The first defines the type of entity, while the second specifies the identifier of a particular deal or invoice.

#|
|| **CRM Object** | **`ownerTypeId`** | **Where to Get `ownerId`** ||
|| Deal | `2` | [crm.deal.list](../../deals/crm-deal-list.md) ||
|| Invoice | `31` | [crm.item.list](../crm-item-list.md) with `entityTypeId = 31` ||
|#

## Getting Started

1. Obtain the order identifier `orderId` using the [sale.order.add](../../../sale/order/sale-order-add.md) or [sale.order.list](../../../sale/order/sale-order-list.md) methods.
2. Prepare the `ownerTypeId` and `ownerId` pair according to the table above.
3. Check the composition of the link fields using the [crm.orderentity.getFields](./crm-order-entity-get-fields.md) method.
4. Create the link using the [crm.orderentity.add](./crm-order-entity-add.md) method.
5. Check the result using the [crm.orderentity.list](./crm-order-entity-list.md) method.
6. If the link is no longer needed, delete it using the [crm.orderentity.deleteByFilter](./crm-order-entity-delete-by-filter.md) method.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#|
|| **Method** | **Description** ||
|| [crm.orderentity.add](./crm-order-entity-add.md) | Creates a link between an order and a CRM object ||
|| [crm.orderentity.list](./crm-order-entity-list.md) | Returns a list of links between orders and CRM entities ||
|| [crm.orderentity.deleteByFilter](./crm-order-entity-delete-by-filter.md) | Deletes a link between an order and a CRM object ||
|| [crm.orderentity.getFields](./crm-order-entity-get-fields.md) | Returns the fields of the order link ||
|#

## Continue Learning

- [{#T}](../index.md)
- [{#T}](../invoice.md)
- [{#T}](../../../sale/order/index.md)