# Data Storage: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Data storage helps applications retain their own information in Bitrix24. You can save entities in the storage, group them by sections, and configure access permissions.

The group of methods `entity.*` allows you to create storages, retrieve their list, modify parameters, manage permissions, and delete unnecessary ones.

> Quick navigation: [all methods](#all-methods)

{% note info "" %}

Methods in this section work only in the context of an [application](../../../settings/app-installation/index.md).

{% endnote %}

## Getting Started

1. Create a new storage using the [entity.add](./entity-add.md) method.

2. Retrieve the list of storages and their identifiers using the [entity.get](./entity-get.md) method.

3. Modify the storage parameters using the [entity.update](./entity-update.md) method.

4. If necessary, view or change access permissions for the storage using the [entity.rights](./entity-rights.md) method.

5. Delete the unnecessary storage using the [entity.delete](./entity-delete.md) method.

## Working with Access Permissions

Access permissions can be set when creating the storage, modified along with its parameters, or updated separately.

#| 
|| **Scenario** | **What to use** ||
|| Create a storage with the required permissions right away | `ACCESS` parameter in [entity.add](./entity-add.md) ||
|| Change storage parameters and access permissions in one call | `ACCESS` parameter in [entity.update](./entity-update.md) ||
|| Retrieve or change only access permissions | [entity.rights](./entity-rights.md) method ||
|#

## Relationships with Other Objects

**Storage Sections.** After creating a storage, you can group entities by sections. Use the [entity.section.*](../sections/index.md) methods to work with sections.

**Storage Entities.** The main application data is stored in entities. Use the [entity.item.*](../items/index.md) methods to work with them.

**Entity Properties.** You can create additional fields for storage entities. Use the [entity.item.property.*](../items/properties/index.md) methods to work with properties.

## Overview of Methods {#all-methods}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#| 
|| **Method** | **Description** ||
|| [entity.add](./entity-add.md) | Adds a data storage ||
|| [entity.update](./entity-update.md) | Updates the parameters of the data storage ||
|| [entity.get](./entity-get.md) | Retrieves the parameters of the data storage or the list of application storages ||
|| [entity.delete](./entity-delete.md) | Deletes the data storage ||
|| [entity.rights](./entity-rights.md) | Retrieves or changes access permissions for the data storage ||
|#