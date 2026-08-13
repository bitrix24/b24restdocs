# Data Storage: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Data storages allow developers to create applications that extend the capabilities of Bitrix24. They can store a variety of information: client records, inventory details, product information, and other data.

Each storage represents an [information block](*iblock) and is available after registering the application in Bitrix24. There is no visual interface for the storage.

> Quick navigation: [all methods](#all-methods)

{% note info "" %}

Methods in this section work only in the context of an [application](../../settings/app-installation/index.md). An application works only with its own storages and does not see the storages of other applications.

{% endnote %}

## Data Structure

**Storages**. Containers for application data. Each storage has a symbolic identifier, a name, and access permissions. Storages can be managed using the methods [entity.*](./entities/index.md).

**Sections**. Designed for grouping data and building a convenient hierarchy. Sections can be managed using the methods [entity.section.*](./sections/index.md).

**Items**. Store application data. Items can be created, modified, and deleted using the methods [entity.item.*](./items/index.md).

**Item Properties**. Define additional fields for items. Any storage allows you to configure such properties using the methods [entity.item.property.*](./items/properties/index.md).

## Getting Started

1. Create a storage using the [entity.add](./entities/entity-add.md) method — specify the `ENTITY` identifier, a name, and access permissions

2. Define additional fields for items using the [entity.item.property.add](./items/properties/entity-item-property-add.md) method

3. If the data needs to be grouped, create sections using the [entity.section.add](./sections/entity-section-add.md) method

4. Add items using the [entity.item.add](./items/entity-item-add.md) method

5. Retrieve the data using the [entity.item.get](./items/entity-item-get.md) and [entity.section.get](./sections/entity-section-get.md) methods

## Storage Identifier {#entity-id}

`ENTITY` is the symbolic identifier of the storage. It is set at creation and passed to every method in this section.

Requirements for the identifier:

- the characters `a-z`, `A-Z`, `0-9`, and `_` are allowed, for example `dish`
- the maximum length is calculated dynamically using the formula `50 - strlen("APP_<clientId>_")` — in most cases for Bitrix24 this is 13 characters
- the identifier is unique within the application

To retrieve the list of application storages and their identifiers, use the [entity.get](./entities/entity-get.md) method. To rename a storage, use the `ENTITY_NEW` parameter of the [entity.update](./entities/entity-update.md) method.

## Access Permission Levels {#access-levels}

Access permissions are set in the format `{"access_code":"permission_level"}`, for example `{"U1":"W","AU":"R"}`. Standard Bitrix24 access codes are used:

- `U<id>` — a user, for example `U1`
- `G<id>` — a user group, for example `G2`
- `AU` — all authorized users

To check the name of an access code, use the [access.name](../common/system/access-name.md) method.

#|
|| **Level** | **What it allows** ||
|| `R` | Read the sections and items of the storage ||
|| `W` | Read the data, as well as create, modify, and delete sections and items ||
|| `X` | All actions of the `W` level, as well as modifying and deleting the storage itself, managing its access permissions and item properties ||
|#

The methods do not accept other levels — such permission records are not added. The user who creates a storage or changes its permissions is always granted the `X` level.

Permissions are managed through the `ACCESS` parameter of the [entity.add](./entities/entity-add.md) and [entity.update](./entities/entity-update.md) methods, or through the separate [entity.rights](./entities/entity-rights.md) method.

## Overview of Methods {#all-methods}

> Scope: [`entity`](../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Storages

#|
|| **Method** | **Description** ||
|| [entity.add](./entities/entity-add.md) | Adds a data storage ||
|| [entity.update](./entities/entity-update.md) | Updates the parameters of the data storage ||
|| [entity.get](./entities/entity-get.md) | Retrieves the parameters of the data storage or the list of application storages ||
|| [entity.delete](./entities/entity-delete.md) | Deletes the data storage ||
|| [entity.rights](./entities/entity-rights.md) | Retrieves or changes access permissions for the data storage ||
|#

### Sections

#|
|| **Method** | **Description** ||
|| [entity.section.add](./sections/entity-section-add.md) | Adds a data storage section ||
|| [entity.section.update](./sections/entity-section-update.md) | Updates a data storage section ||
|| [entity.section.get](./sections/entity-section-get.md) | Retrieves the list of data storage sections ||
|| [entity.section.delete](./sections/entity-section-delete.md) | Deletes a data storage section ||
|#

### Items

#|
|| **Method** | **Description** ||
|| [entity.item.add](./items/entity-item-add.md) | Adds a data storage item ||
|| [entity.item.update](./items/entity-item-update.md) | Updates a data storage item ||
|| [entity.item.get](./items/entity-item-get.md) | Retrieves the list of data storage items ||
|| [entity.item.delete](./items/entity-item-delete.md) | Deletes a data storage item ||
|#

### Item Properties

#|
|| **Method** | **Description** ||
|| [entity.item.property.add](./items/properties/entity-item-property-add.md) | Adds a property for data storage items ||
|| [entity.item.property.update](./items/properties/entity-item-property-update.md) | Updates a property for data storage items ||
|| [entity.item.property.get](./items/properties/entity-item-property-get.md) | Retrieves the list of properties for data storage items ||
|| [entity.item.property.delete](./items/properties/entity-item-property-delete.md) | Deletes a property for data storage items ||
|#

[*iblock]: An information block is a special object for storing news, services, articles, product catalogs, and other data that has a clear structure.