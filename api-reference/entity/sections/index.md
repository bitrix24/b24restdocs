# Data Storage Sections: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Data storage sections help group elements within the application and build a hierarchy.

The group of methods `entity.section.*` allows you to create sections, retrieve their list, modify parameters, and delete unnecessary ones.

> Quick Navigation: [All Methods](#all-methods)

{% note info "" %}

Section methods only work in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}

## Getting Started

1. Obtain the storage identifier using the [entity.get](../entities/entity-get.md) method.
2. If a hierarchy is needed in the storage, create the first section using the [entity.section.add](./entity-section-add.md) method.
3. Retrieve the list of sections and their identifiers using the [entity.section.get](./entity-section-get.md) method.
4. Modify the section parameters using the [entity.section.update](./entity-section-update.md) method.
5. Delete the unnecessary section using the [entity.section.delete](./entity-section-delete.md) method.

## Relationship with Other Objects

**Data Storage.** Each method in the group works with a specific data storage of the application. For this, the `ENTITY` parameter is passed to the methods — the identifier of the storage. You can obtain it using the [entity.get](../entities/entity-get.md) method.

**Storage Elements.** Sections are used to group elements. To work with elements, use the [entity.item.*](../items/index.md) methods.

**Parent Section.** In the [entity.section.add](./entity-section-add.md) and [entity.section.update](./entity-section-update.md) methods, you can specify the `SECTION` parameter to create a nested section or move a section inside another.

## Overview of Methods {#all-methods}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#| 
|| **Method** | **Description** ||
|| [entity.section.add](./entity-section-add.md) | Adds a data storage section ||
|| [entity.section.update](./entity-section-update.md) | Updates a data storage section ||
|| [entity.section.get](./entity-section-get.md) | Retrieves the list of data storage sections ||
|| [entity.section.delete](./entity-section-delete.md) | Deletes a data storage section ||
|#