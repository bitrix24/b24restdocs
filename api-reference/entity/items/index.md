# Data Storage Elements: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Data storage elements hold application information. They can store core fields, property values, and links to sections.

The group of methods `entity.item.*` allows you to create elements, retrieve their list, modify parameters, and delete unnecessary ones.

> Quick navigation: [all methods](#all-methods)

{% note info "" %}

Methods in this section work only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}

## Getting Started

1. Obtain the storage identifier using the [entity.get](../entities/entity-get.md) method.

2. If sections are used in the storage, get the section identifier using the [entity.section.get](../sections/entity-section-get.md) method.

3. If additional fields are needed for the elements, create properties or retrieve a list of existing properties and their codes using the [entity.item.property.*](./properties/index.md) methods.

4. Create the first element using the [entity.item.add](./entity-item-add.md) method.

5. Retrieve the list of elements and their identifiers using the [entity.item.get](./entity-item-get.md) method.

6. Modify the element parameters using the [entity.item.update](./entity-item-update.md) method.

7. Delete the unnecessary element using the [entity.item.delete](./entity-item-delete.md) method.

## Relationship with Other Objects

**Data Storage.** Each method in the group works with a specific data storage of the application. To work with storages, use the [entity.*](../entities/index.md) methods.

**Storage Sections.** An element can be linked to a section via the `SECTION` parameter. To work with sections, use the [entity.section.*](../sections/index.md) methods.

**Element Properties.** Additional data for the element is passed in the `PROPERTY_VALUES` parameter. To work with properties, use the [entity.item.property.*](./properties/index.md) methods.

## Overview of Methods {#all-methods}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#| 
|| **Method** | **Description** ||
|| [entity.item.add](./entity-item-add.md) | Adds a data storage element ||
|| [entity.item.update](./entity-item-update.md) | Updates a data storage element ||
|| [entity.item.get](./entity-item-get.md) | Retrieves a list of data storage elements ||
|| [entity.item.delete](./entity-item-delete.md) | Deletes a data storage element ||
|#