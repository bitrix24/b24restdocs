# SPA Types in REST 3.0: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

These methods belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

An SPA type is a configurable CRM object with its own items, for example, "Supplier Requests" or "Contracts". The methods in this group create types, update their settings, and return a list of types with their `entityTypeId` IDs. The `entityTypeId` is part of the item method names: items of type `1038` are created by the [crm.smartprocess1038.add](../items/crm-entity-add-rest-v3.md) method.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Smart Process Automation in CRM](https://helpdesk.bitrix24.com/open/19141012/)

## How to Get Started

Example: an app keeps track of supplier requests in its own SPA.

1. Check whether the required type exists with the [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md) method filtered by `code` or `title`. If the type exists, take its `entityTypeId` and go to step 3
2. Create a type with the [crm.smartprocess.add](./crm-smart-process-add-rest-v3.md) method. Pass the name in `title` and a symbolic code in `code` so that you can find the type by it later, and enable the features you need, for example, pipelines and stages with `isCategoriesEnabled` and `isStagesEnabled`. The method returns the `entityTypeId` of the new type
3. Add custom fields to the type with the [crm.smartprocess&lt;entityTypeId&gt;.field.custom.add](../fields/crm-entity-field-custom-add-rest-v3.md) method. The field types you can create are returned by the [crm.dictionary.customfieldtype.list](../dictionaries/crm-dictionary-list-rest-v3.md#custom-field-types) method
4. Configure pipelines and stages with the [crm.smartprocess&lt;entityTypeId&gt;.category.*](../categories/index.md) methods
5. Create items with the [crm.smartprocess&lt;entityTypeId&gt;.add](../items/crm-entity-add-rest-v3.md) method
6. Update the type settings with the [crm.smartprocess.update](./crm-smart-process-update-rest-v3.md) method. The method changes only the fields that are passed

## Specifics {#limits}

**Type ID.** The methods in this group find a type by `entityTypeId`, not by the `id` from the previous-version method [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md). Bitrix24 assigns the `entityTypeId` value itself when the type is created; you cannot set it.

**System types.** The methods work only with SPA types. Deals, invoices, documents, and other system types are not included in the [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md) response, and [crm.smartprocess.get](./crm-smart-process-get-rest-v3.md) returns a "not found" error for their `entityTypeId`. System type IDs are returned by the [crm.dictionary.entitytype.list](../dictionaries/crm-dictionary-list-rest-v3.md#entity-types) method.

**Access.** A type can be read by a CRM administrator, an administrator of this SPA, a user with permission to read items of the type, or a user with permission to manage SPAs. The [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md) method skips types the user has no access to. Types can be created and updated by a CRM administrator or a user with permission to manage SPAs.

**What the write methods set.** The [crm.smartprocess.add](./crm-smart-process-add-rest-v3.md) and [crm.smartprocess.update](./crm-smart-process-update-rest-v3.md) methods change the name, code, description, item completion deadline, feature flags, `relations` links, `linkedUserFields` bindings, and `primaryCustomSectionId` and `customSectionIds` digital workplaces. These methods do not change the `isPaymentsEnabled` payment acceptance flag.

**Deletion.** The [crm.smartprocess.delete](./crm-smart-process-delete-rest-v3.md) method deletes a type by `entityTypeId`. Before deleting a type, delete its items. Bitrix24 removes links to other types and placement in digital workplaces along with the type.

**Method names.** Names are written in lowercase: `crm.smartprocess.list`. For `crm.smartProcess.list`, Bitrix24 returns the "Method not found" error.

## Relationship with Other Objects

An SPA type is linked to items, fields, pipelines, and other CRM types.

**Items.** Items of the type are created, retrieved, and updated by the [crm.smartprocess&lt;entityTypeId&gt;.*](../items/index.md) methods. The set of item fields depends on the type settings: for example, without `isCategoriesEnabled`, items have no `categoryId` field. What depends on each setting is shown on the [SPA fields](../items/fields-smart-process.md) page.

**Fields.** Item field descriptions are returned by the [crm.smartprocess&lt;entityTypeId&gt;.field.list](../fields/crm-entity-field-list-rest-v3.md) method, and custom fields are created by the [crm.smartprocess&lt;entityTypeId&gt;.field.custom.add](../fields/crm-entity-field-custom-add-rest-v3.md) method. Do not confuse them with the [crm.smartprocess.field.list](./crm-smart-process-field-list-rest-v3.md) and [crm.smartprocess.field.get](./crm-smart-process-field-get-rest-v3.md) methods: they describe the fields of the type itself.

**Pipelines and stages.** If `isCategoriesEnabled` and `isStagesEnabled` are enabled for the type, pipelines and stages are managed by the [crm.smartprocess&lt;entityTypeId&gt;.category.*](../categories/index.md) methods.

**Other CRM types.** The `relations` object in the [crm.smartprocess.get](./crm-smart-process-get-rest-v3.md#relations) response shows which types an SPA item can be linked to and which items can be linked to it.

**Digital workplaces.** The `primaryCustomSectionId` and `customSectionIds` fields specify the primary and other digital workplaces of the type. The [crm.automatedsolution.*](../../automated-solution/index.md) methods return the workplace directory.

## Methods of the Previous Version

At the `/rest/` address, SPA types are handled by the [crm.type.*](../../universal/user-defined-object-types/index.md) methods. In REST 3.0, flags are passed as `true` and `false`, not `Y` and `N`, a type is found by `entityTypeId`, and `select`, `filter`, `order`, and `pagination` are written in the [REST 3.0](../../../rest-v3.md) format. The previous-version method [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) is needed to find out the type `id`: it is part of custom field names of the form `UF_CRM_<id>_<code>`.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### SPA Types

#|
|| **Method** | **Description** ||
|| [crm.smartprocess.add](./crm-smart-process-add-rest-v3.md) | Creates an SPA type ||
|| [crm.smartprocess.update](./crm-smart-process-update-rest-v3.md) | Updates the settings of an SPA type ||
|| [crm.smartprocess.delete](./crm-smart-process-delete-rest-v3.md) | Deletes a type that has no items ||
|| [crm.smartprocess.get](./crm-smart-process-get-rest-v3.md) | Returns the settings of an SPA type by `entityTypeId` ||
|| [crm.smartprocess.list](./crm-smart-process-list-rest-v3.md) | Returns SPA types with filtering, sorting, and pagination ||
|#

### Type Field Descriptions

#|
|| **Method** | **Description** ||
|| [crm.smartprocess.field.get](./crm-smart-process-field-get-rest-v3.md) | Returns the description of an SPA type field by name ||
|| [crm.smartprocess.field.list](./crm-smart-process-field-list-rest-v3.md) | Returns descriptions of SPA type fields ||
|#
