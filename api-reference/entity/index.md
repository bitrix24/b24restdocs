# Data Storage: Overview of Methods

Data storages allow developers to create applications that extend the capabilities of Bitrix24. They can store a variety of information: client records, inventory details, product information, and other data.

Each storage represents an [information block](*iblock) and is available after registering the application in Bitrix24. There is no visual interface for the storage.

Application storages can be managed using a group of methods [entity.*](./entities/index.md).

> Quick navigation: [all methods](#all-methods)

## Storage Structure

**Sections**. Designed for grouping data and building a convenient hierarchy. Sections can be managed using the methods [entity.section.*](./sections/index.md).

**Items**. Store application data. Items can be created, modified, and deleted using the methods [entity.item.*](./items/index.md).

## Additional Properties

Any storage allows you to configure additional properties for items. To manage them, use the methods [entity.item.property.*](./items/properties/index.md).

## Overview of Methods {#all-methods}

> Scope: [`entity`](../scopes/permissions.md)
>
> Who can execute the method: any user

### Storages

#|
|| **Method** | **Description** ||
|| [entity.add](./entities/entity-add.md) | Creates a data storage ||
|| [entity.update](./entities/entity-update.md) | Modifies the parameters of the data storage ||
|| [entity.rights](./entities/entity-rights.md) | Gets or changes access permissions for the storage ||
|| [entity.get](./entities/entity-get.md) | Retrieves the parameters of the storage or a list of all application storages ||
|| [entity.delete](./entities/entity-delete.md) | Deletes the storage ||
|#

### Sections

#|
|| **Method** | **Description** ||
|| [entity.section.get](./sections/entity-section-get.md) | Retrieves a list of storage sections ||
|| [entity.section.add](./sections/entity-section-add.md) | Adds a storage section ||
|| [entity.section.update](./sections/entity-section-update.md) | Modifies a storage section ||
|| [entity.section.delete](./sections/entity-section-delete.md) | Deletes a storage section ||
|#

### Items

#|
|| **Method** | **Description** ||
|| [entity.item.get](./items/entity-item-get.md) | Retrieves a list of storage items ||
|| [entity.item.add](./items/entity-item-add.md) | Adds a storage item ||
|| [entity.item.update](./items/entity-item-update.md) | Modifies a storage item ||
|| [entity.item.delete](./items/entity-item-delete.md) | Deletes a storage item ||
|#

### Item Properties

#|
|| **Method** | **Description** ||
|| [entity.item.property.get](./items/properties/entity-item-property-get.md) | Retrieves a list of additional properties for storage items ||
|| [entity.item.property.add](./items/properties/entity-item-property-add.md) | Adds an additional property for storage items ||
|| [entity.item.property.update](./items/properties/entity-item-property-update.md) | Modifies an additional property for storage items ||
|| [entity.item.property.delete](./items/properties/entity-item-property-delete.md) | Deletes an additional property for storage items ||
|#

[*iblock]: An information block is a special object for storing news, services, articles, product catalogs, and other data that has a clear structure.