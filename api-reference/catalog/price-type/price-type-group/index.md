# Binding Price Types to Customer Groups: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Price type bindings to customer groups manage group access to price types in the Trade Catalog. A binding determines whether a customer group can only view the price or purchase the product at that price type.

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [How to Set Access Permissions for the Product Catalog](https://helpdesk.bitrix24.com/open/25386568/)

## How to Start

1. Obtain the identifiers `catalogGroupId` and `groupId`, then select the access type `access`
2. If necessary, check the available fields and their types using [catalog.priceTypeGroup.getFields](./catalog-price-type-group-get-fields.md)
3. Create the binding using [catalog.priceTypeGroup.add](./catalog-price-type-group-add.md)
4. Verify the result using [catalog.priceTypeGroup.list](./catalog-price-type-group-list.md)
5. If the binding is no longer needed, delete it using [catalog.priceTypeGroup.delete](./catalog-price-type-group-delete.md)

## Bindings in REST 3.0

In [REST 3.0](../../../rest-v3.md), bindings have methods for adding and deleting, as well as the `catalog.pricetype.group.field.list` and `catalog.pricetype.group.field.get` methods for retrieving the field schema. Methods available in both API versions are marked `v 3.0` in the table.

Specifics of the new version methods:

- the new version has no method for retrieving a list of bindings. The binding identifier is returned by the [catalog.pricetype.group.add](./catalog-price-type-group-add-rest-v3.md) method or by the previous version method [catalog.priceTypeGroup.list](./catalog-price-type-group-list.md)
- a binding cannot be updated. To change the group or the access type, delete the binding and create a new one
- when a price type is created, Bitrix24 automatically adds two bindings to group `1`: for viewing and for purchasing. Adding the same binding again returns an error
- method names are written in lowercase: `catalog.pricetype.group.add`

## Relationships with Other Objects

**Price Type.** The binding is created for a specific price type in the `catalogGroupId` field. You can obtain the price type identifier using the methods [catalog.priceType.list](../catalog-price-type-list.md) and [catalog.priceType.get](../catalog-price-type-get.md).

**Customer Group.** The binding is created for a specific customer group in the `groupId` field. For existing bindings, `groupId` can be retrieved using the method [catalog.priceTypeGroup.list](./catalog-price-type-group-list.md).

**Access Type.** The `access` field defines the rights of the group to the price type.

Available values:

- `Y` — the group can purchase at the price type
- `N` — the group can only view the price type

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method

#|
|| **Method** | **Description** ||
|| [catalog.priceTypeGroup.add](./catalog-price-type-group-add.md) | Adds a price type binding to a customer group ||
|| [catalog.pricetype.group.add](./catalog-price-type-group-add-rest-v3.md) | Adds a price type binding to a customer group v 3.0 ||
|| [catalog.priceTypeGroup.list](./catalog-price-type-group-list.md) | Returns a list of price type bindings to customer groups ||
|| [catalog.priceTypeGroup.delete](./catalog-price-type-group-delete.md) | Deletes a price type binding from a customer group ||
|| [catalog.pricetype.group.delete](./catalog-price-type-group-delete-rest-v3.md) | Deletes a price type binding from a customer group v 3.0 ||
|| [catalog.priceTypeGroup.getFields](./catalog-price-type-group-get-fields.md) | Returns the fields of price type bindings to customer groups ||
|| [catalog.pricetype.group.field.list](./catalog-price-type-group-field-list.md) | Returns a list of price type binding fields v 3.0 ||
|| [catalog.pricetype.group.field.get](./catalog-price-type-group-field-get.md) | Returns the description of a price type binding field ||
|#
