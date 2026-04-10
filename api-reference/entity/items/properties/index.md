# Properties of Data Storage Elements: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The properties of data storage elements help store additional data within application elements. They allow for the definition of custom fields.

The group of methods `entity.item.property.*` enables the creation of properties, retrieval of property lists, modification of parameters, and deletion of unnecessary properties.

> Quick Navigation: [All Methods](#all-methods)

{% note info "" %}

The methods in this section work only within the context of the [application](../../../../settings/app-installation/index.md).

{% endnote %}

## Getting Started

1. Obtain the storage identifier using the [entity.get](../../entities/entity-get.md) method.
2. Create a new property using the [entity.item.property.add](./entity-item-property-add.md) method.
3. Retrieve the list of properties and their codes using the [entity.item.property.get](./entity-item-property-get.md) method.
4. Modify the property parameters using the [entity.item.property.update](./entity-item-property-update.md) method.
5. Delete the unnecessary property using the [entity.item.property.delete](./entity-item-property-delete.md) method.

## Relationship with Other Objects

**Data Storage.** Each method in the group operates with a specific data storage of the application. The `ENTITY` parameter, which is its identifier, is passed to the methods. This can be obtained using the [entity.get](../../entities/entity-get.md) method.

**Storage Elements.** Properties define additional fields for data storage elements. To work with them, use the [entity.item.*](../index.md) methods.

## Overview of Methods {#all-methods}

> Scope: [`entity`](../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [entity.item.property.add](./entity-item-property-add.md) | Adds a property to data storage elements ||
|| [entity.item.property.update](./entity-item-property-update.md) | Updates a property of data storage elements ||
|| [entity.item.property.get](./entity-item-property-get.md) | Retrieves a list of properties for data storage elements ||
|| [entity.item.property.delete](./entity-item-property-delete.md) | Deletes a property from data storage elements ||
|#