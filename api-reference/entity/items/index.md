# Data Storage Items: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Data storage items hold application information. They can store core fields, property values, and links to sections.

The group of methods `entity.item.*` allows you to create items, retrieve their list, modify parameters, and delete unnecessary ones.

> Quick navigation: [all methods](#all-methods)

{% note info "" %}

Methods in this section work only in the context of the [application](../../../settings/app-installation/index.md).

{% endnote %}

## Getting Started

1. Obtain the storage identifier using the [entity.get](../entities/entity-get.md) method.

2. If sections are used in the storage, get the section identifier using the [entity.section.get](../sections/entity-section-get.md) method.

3. If additional fields are needed for the items, create properties or retrieve a list of existing properties and their codes using the [entity.item.property.*](./properties/index.md) methods.

4. Create the first item using the [entity.item.add](./entity-item-add.md) method.

5. Retrieve the list of items and their identifiers using the [entity.item.get](./entity-item-get.md) method.

6. Modify the item parameters using the [entity.item.update](./entity-item-update.md) method.

7. Delete the unnecessary item using the [entity.item.delete](./entity-item-delete.md) method.

## What an Item Can Store

**Core fields.** The item name and activity dates. They are passed as separate parameters of the [entity.item.add](./entity-item-add.md) and [entity.item.update](./entity-item-update.md) methods.

**Property values.** Additional data is passed in the `PROPERTY_VALUES` parameter in the format `{"PROPERTY_CODE": value}`. Only the properties already created in the storage using the [entity.item.property.add](./properties/entity-item-property-add.md) method are processed.

**Custom fields.** The `UF_*` fields are passed as separate parameters in the format `"UF_CODE": value`, for example `"UF_CRM_1_COLOR": "red"`.

**Files.** The preview picture, the detail picture, and the values of properties of the `F` (file) type are passed in the format described in the article [How to Upload Files](../../files/how-to-upload-files.md). In the response of the [entity.item.get](./entity-item-get.md) method, such fields contain a link to the file.

## How to Retrieve Lists of Items

The [entity.item.get](./entity-item-get.md) method returns items page by page: the page size is fixed at `50` records, and the next page is requested using the `start` parameter. Selection and sorting are set by the `FILTER` and `SORT` parameters, and comparison prefixes such as `>=`, `%`, or `!` are supported in the filter keys.

For more details on pagination, refer to the article [Features of List Methods](../../../settings/how-to-call-rest-api/list-methods-pecularities.md).

## Relationship with Other Objects

**Data Storage.** Each method in the group works with a specific data storage of the application. The `ENTITY` parameter, which is its identifier, is passed to the methods. The requirements for the identifier are described in the [Storage Identifier](../index.md#entity-id) section, and the value can be obtained using the [entity.get](../entities/entity-get.md) method. The permission level required to work with items is described in the [Access Permission Levels](../index.md#access-levels) section.

**Storage Sections.** An item can be linked to a section via the `SECTION` parameter. To work with sections, use the [entity.section.*](../sections/index.md) methods.

**Item Properties.** Additional fields for an item are created in advance using the [entity.item.property.*](./properties/index.md) methods. The list of available codes and their types is returned by the [entity.item.property.get](./properties/entity-item-property-get.md) method.

## Overview of Methods {#all-methods}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#| 
|| **Method** | **Description** ||
|| [entity.item.add](./entity-item-add.md) | Adds a data storage item ||
|| [entity.item.update](./entity-item-update.md) | Updates a data storage item ||
|| [entity.item.get](./entity-item-get.md) | Retrieves a list of data storage items ||
|| [entity.item.delete](./entity-item-delete.md) | Deletes a data storage item ||
|#