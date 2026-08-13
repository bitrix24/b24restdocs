# Properties of Data Storage Items: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The properties of data storage items help store additional data within application items. They allow for the definition of custom fields.

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

## Property Types

The type is set by the `TYPE` parameter when a property is created.

#|
|| **Type** | **What it stores** | **How to pass the value** ||
|| `S` | A string | A string value in the `PROPERTY_VALUES` parameter of the [entity.item.add](../entity-item-add.md) method ||
|| `N` | A number | A numeric value in the `PROPERTY_VALUES` parameter ||
|| `F` | A file | A file in the format described in the article [How to Upload Files](../../../files/how-to-upload-files.md). In the response of the [entity.item.get](../entity-item-get.md) method, the property contains a link to the file ||
|#

The methods in this section do not accept other types, including the list type `L` — such a call returns the error `Wrong entity item property type`.

## Specifics of Working with Properties

**Property code.** `PROPERTY` is the symbolic code used to pass the property to items. The characters `a-z`, `A-Z`, `0-9`, and `_` are allowed. The code is unique within the storage.

**Renaming.** The property code is changed using the `PROPERTY_NEW` parameter of the [entity.item.property.update](./entity-item-property-update.md) method. After renaming, the new code is used in the `PROPERTY_VALUES` parameter of the items.

**Changing the type.** The type of an existing property can be changed, but it cannot be set to `F`. To get a property of the file type, create a new property using the [entity.item.property.add](./entity-item-property-add.md) method.

**Deletion.** The [entity.item.property.delete](./entity-item-property-delete.md) method deletes the property along with its values in all items of the storage.

## Relationship with Other Objects

**Data Storage.** Each method in the group operates with a specific data storage of the application. The `ENTITY` parameter, which is its identifier, is passed to the methods. The requirements for the identifier are described in the [Storage Identifier](../../index.md#entity-id) section, and the value can be obtained using the [entity.get](../../entities/entity-get.md) method. Properties can be managed by a user with the `X` permission level — the levels are described in the [Access Permission Levels](../../index.md#access-levels) section.

**Storage Items.** Properties define additional fields for data storage items. Property values are passed in the `PROPERTY_VALUES` parameter of the [entity.item.*](../index.md) methods. Only the codes already created in the storage are processed.

## Overview of Methods {#all-methods}

> Scope: [`entity`](../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [entity.item.property.add](./entity-item-property-add.md) | Adds a property to data storage items ||
|| [entity.item.property.update](./entity-item-property-update.md) | Updates a property of data storage items ||
|| [entity.item.property.get](./entity-item-property-get.md) | Retrieves a list of properties for data storage items ||
|| [entity.item.property.delete](./entity-item-property-delete.md) | Deletes a property from data storage items ||
|#