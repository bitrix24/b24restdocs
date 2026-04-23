# Linking Orders to CRM Entities: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Online store orders can be linked to CRM entities. This allows you to use order information in workflows related to deals and invoices.

> Quick Navigation: [all methods](#all-methods)

## Linking to Other Entities

**Online Store Orders.** The order identifier `orderId` links the order record to a CRM entity.

**CRM Entities.** The linking is supported for deals and invoices using the parameter pair `ownerTypeId` and `ownerId`. The first defines the type of entity, while the second specifies the identifier of a particular deal or invoice.

## Getting Started

1. Obtain the order identifier `orderId` using the [sale.order.add](../../../sale/order/sale-order-add.md) or [sale.order.list](../../../sale/order/sale-order-list.md) methods.
2. Determine the entity type `ownerTypeId` using the [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) method: `2` for deals, `31` for invoices.
3. Retrieve `ownerId` using the [crm.deal.list](../../deals/crm-deal-list.md) method for deals or [crm.item.list](../crm-item-list.md) with `entityTypeId = 31` for invoices.
4. Create the link using the [crm.orderentity.add](./crm-order-entity-add.md) method.
5. Check the result using the [crm.orderentity.list](./crm-order-entity-list.md) method.
6. If necessary, remove the link using the [crm.orderentity.deleteByFilter](./crm-order-entity-delete-by-filter.md) method.
7. To obtain the structure of the link fields, use [crm.orderentity.getFields](./crm-order-entity-get-fields.md).

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#|
|| **Method** | **Description** ||
|| [crm.orderentity.add](./crm-order-entity-add.md) | Creates a link between an order and a CRM entity ||
|| [crm.orderentity.list](./crm-order-entity-list.md) | Returns a list of links between orders and CRM entities ||
|| [crm.orderentity.deleteByFilter](./crm-order-entity-delete-by-filter.md) | Deletes a link between an order and a CRM entity ||
|| [crm.orderentity.getFields](./crm-order-entity-get-fields.md) | Returns the fields of the order link ||
|#