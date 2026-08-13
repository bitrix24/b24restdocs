# Data Storage Sections: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Data storage sections help group items within the application and build a hierarchy.

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

## What Can Be Set in a Section

**Core fields.** The name, symbolic code, description, activity flag, and sorting index. They are passed as separate parameters of the [entity.section.add](./entity-section-add.md) and [entity.section.update](./entity-section-update.md) methods.

**Custom fields.** The `UF_*` fields are passed as separate parameters in the format `"UF_CODE": value`, for example `"UF_COLOR": "#ff6600"`.

**Files.** The section picture and detail picture are passed in the format described in the article [How to Upload Files](../../files/how-to-upload-files.md). In the response of the [entity.section.get](./entity-section-get.md) method, such fields contain a link to the file.

## How to Retrieve Lists of Sections

The [entity.section.get](./entity-section-get.md) method returns sections page by page: the page size is fixed at `50` records, and the next page is requested using the `start` parameter. Selection and sorting are set by the `FILTER` and `SORT` parameters, and comparison prefixes such as `>=`, `%`, or `!` are supported in the filter keys.

For more details on pagination, refer to the article [Features of List Methods](../../../settings/how-to-call-rest-api/list-methods-pecularities.md).

## Relationship with Other Objects

**Data Storage.** Each method in the group works with a specific data storage of the application. For this, the `ENTITY` parameter is passed to the methods — the identifier of the storage. The requirements for the identifier are described in the [Storage Identifier](../index.md#entity-id) section, and the value can be obtained using the [entity.get](../entities/entity-get.md) method. The permission level required to work with sections is described in the [Access Permission Levels](../index.md#access-levels) section.

**Storage Items.** Sections are used to group items. An item is linked to a section using the `SECTION` parameter. To work with items, use the [entity.item.*](../items/index.md) methods.

**Parent Section.** In the [entity.section.add](./entity-section-add.md) and [entity.section.update](./entity-section-update.md) methods, you can specify the `SECTION` parameter to create a nested section or move a section inside another. A top-level section is created without this parameter.

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