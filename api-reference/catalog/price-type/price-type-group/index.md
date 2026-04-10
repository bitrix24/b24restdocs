# Binding Price Types to Customer Groups: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This section describes the access rights of customer groups to price types in the Trade Catalog. The binding determines whether a customer group can only view the price or purchase the product at that price type.

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [How to Set Access Permissions for the Product Catalog](https://helpdesk.bitrix24.com/open/25386568/)

## Connection with Other Objects

**Price Type.** The binding is created for a specific price type in the `catalogGroupId` field. You can obtain the price type identifier using the methods [catalog.priceType.list](../catalog-price-type-list.md) and [catalog.priceType.get](../catalog-price-type-get.md).

**Customer Group.** The binding is created for a specific customer group in the `groupId` field. For existing bindings, `groupId` can be retrieved using the method [catalog.priceTypeGroup.list](./catalog-price-type-group-list.md).

**Access Type.** The `access` field defines the rights of the group to the price type.

Available values:

- `Y` — the group can purchase at the price type,
- `N` — the group can only view the price type.

## Procedure for Working with Bindings

1. Obtain the identifiers `catalogGroupId` and `groupId`, then select the access type `access`.
2. If necessary, check the available fields and their types using the method [catalog.priceTypeGroup.getFields](./catalog-price-type-group-get-fields.md).
3. Create the binding using the method [catalog.priceTypeGroup.add](./catalog-price-type-group-add.md).
4. Verify the result using the method [catalog.priceTypeGroup.list](./catalog-price-type-group-list.md).
5. If the binding is no longer needed, delete it using the method [catalog.priceTypeGroup.delete](./catalog-price-type-group-delete.md).

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#| 
|| **Method** | **Description** ||
|| [catalog.priceTypeGroup.add](./catalog-price-type-group-add.md) | Adds a price type binding to a customer group ||
|| [catalog.priceTypeGroup.list](./catalog-price-type-group-list.md) | Returns a list of price type bindings to customer groups ||
|| [catalog.priceTypeGroup.delete](./catalog-price-type-group-delete.md) | Deletes a price type binding from a customer group ||
|| [catalog.priceTypeGroup.getFields](./catalog-price-type-group-get-fields.md) | Returns the fields of price type bindings to customer groups ||
|#