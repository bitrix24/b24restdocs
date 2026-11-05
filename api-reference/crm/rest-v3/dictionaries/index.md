# CRM Directories in REST 3.0: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

These methods belong to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

CRM directories store the values that a user selects in card fields: a deal or company type, a source, a salutation, a phone type. The methods in this group return directory values. In seven directories, values can be added, updated, and deleted. These directories are marked in the table below.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Statuses and Dropdowns in CRM](https://helpdesk.bitrix24.com/open/21656894/)

## Directories

The directory is part of the method name: [crm.dictionary.dealtype.list](./crm-dictionary-list-rest-v3.md) returns deal types, and [crm.dictionary.source.list](./crm-dictionary-list-rest-v3.md) returns sources.

#|
|| **Directory** | **Segment in the method name** | **Values can be changed** ||
|| Deal types | `dealtype` | Yes ||
|| Contact types | `contacttype` | Yes ||
|| Company types | `companytype` | Yes ||
|| Salutations | `honorific` | Yes ||
|| Number of employees | `employees` | Yes ||
|| Industries | `industry` | Yes ||
|| Sources | `source` | Yes ||
|| Phone types | `phonetype` | No ||
|| Email types | `emailtype` | No ||
|| Website types | `webtype` | No ||
|| Messenger types | `imtype` | No ||
|| Address types | `addresstype` | No ||
|| CRM object types | `entitytype` | No ||
|| Stage groups | `stagesemantic` | No ||
|| Activity providers | `activityprovider` | No ||
|#

The method name is written in lowercase: `crm.dictionary.dealtype.list`, not `crm.dictionary.dealType.list`.

Value fields differ between directories. They are described on the page of the [crm.dictionary.&lt;directory&gt;.list](./crm-dictionary-list-rest-v3.md#fields) method.

## Getting Started

Example: an app sells subscriptions and marks such deals with its own "Subscription" type.

1. Retrieve deal types with the [crm.dictionary.dealtype.list](./crm-dictionary-list-rest-v3.md) method. If the value you need already exists, take its `id` and go to step 3
2. If there is no suitable value, add it with the [crm.dictionary.dealtype.add](./crm-dictionary-add-rest-v3.md) method. Pass your own `id`, for example, `SUBSCRIPTION`, and the app will know it in advance. Without `id`, Bitrix24 generates an ID such as `UC_1Z8DXI`
3. Pass the value `id` in the `typeId` field when you create a deal with the [crm.deal.add](../items/crm-entity-add-rest-v3.md) method
4. Rename the value with the [crm.dictionary.dealtype.update](./crm-dictionary-update-rest-v3.md) method. The ID does not change, and deals retain their type
5. Delete the value with the [crm.dictionary.dealtype.delete](./crm-dictionary-delete-rest-v3.md) method when you no longer need it. Deals of this type keep the previous `id` in the `typeId` field. Default values with `isSystem: true` cannot be deleted

## Relationship with Other Objects

Directories are linked to CRM items and their phones, emails, websites, and messengers, as well as to addresses, stages, activities, and object types.

**CRM items.** An item stores the value ID in a field: `typeId` of a deal, contact, or company, `sourceId`, `honorificId`, `industryId`, `employeesId`. If you need the whole directory value rather than only its ID, pass a related object without the `Id` suffix in `select` of the [crm.&lt;type&gt;.get](../items/crm-entity-get-rest-v3.md) method, for example, `type` or `source`. The fields of each object type are described on the fields pages, for example, [deal fields](../items/fields-deal.md) and [company fields](../items/fields-company.md).

**Phones, emails, websites, and messengers.** The `phone`, `email`, `web`, and `im` fields can contain several values. The type of each value is set by the `valueTypeId` key. The types are stored in the [communication type directories](./crm-dictionary-list-rest-v3.md#communication-types): `phonetype`, `emailtype`, `webtype`, `imtype`.

**Addresses.** The type of a contact or company address, for example, a legal or street address, is set by the `typeId` field in the methods of the [crm.&lt;type&gt;.address.*](../addresses/crm-entity-address-list-rest-v3.md) group. Address types are stored in the [`addresstype`](./crm-dictionary-list-rest-v3.md#address-types) directory.

**Stages.** A deal or SPA stage belongs to one of the groups of the [`stagesemantic`](./crm-dictionary-list-rest-v3.md#stage-semantics) directory: in progress, successful, or unsuccessful. Pipeline stages are returned by the [crm.&lt;type&gt;.category.stage.list](../categories/crm-entity-category-stage-list-rest-v3.md) method.

**Activities.** The `providerId` field shows what kind of activity it is: a call, an email, a meeting, or a task. For example, for an email, `providerId` is `CRM_EMAIL`. All providers are stored in the [`activityprovider`](./crm-dictionary-list-rest-v3.md#activity-providers) directory. The activities of an item are returned by the [crm.&lt;type&gt;.activity.list](../activities/crm-entity-activity-list-rest-v3.md) method.

**Object types.** The [`entitytype`](./crm-dictionary-list-rest-v3.md#entity-types) directory stores the `entityTypeId` of system types: lead, deal, contact, company, and others. It contains no SPAs. Their `entityTypeId` is returned by the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method.

## Methods of the Previous Version

At the `/rest/` address, the [crm.status.*](../../status/index.md) methods handle deal, contact, and company types, salutations, numbers of employees, industries, and sources. In them, the directory is set by the `ENTITY_ID` parameter, for example, `SOURCE` or `DEAL_TYPE`, while in REST 3.0 it is set by the method name. Fields there are written in uppercase, for example, `STATUS_ID` and `NAME`, while in REST 3.0 they are written in camelCase. In the previous version, address types are returned by the [crm.enum.addresstype](../../auxiliary/enum/crm-enum-address-type.md) method, and CRM object types by the [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) method.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Editing

These methods are available for the `dealtype`, `contacttype`, `companytype`, `honorific`, `employees`, `industry`, and `source` directories. They can be called by a CRM administrator.

#|
|| **Method** | **Description** ||
|| [crm.dictionary.&lt;directory&gt;.add](./crm-dictionary-add-rest-v3.md) | Adds a value to a directory ||
|| [crm.dictionary.&lt;directory&gt;.update](./crm-dictionary-update-rest-v3.md) | Updates the name and sort order of a value ||
|| [crm.dictionary.&lt;directory&gt;.delete](./crm-dictionary-delete-rest-v3.md) | Deletes a directory value ||
|#

### Reading

These methods are available for all 15 directories. They can be called by a user with permission to view leads, deals, or other CRM objects.

#|
|| **Method** | **Description** ||
|| [crm.dictionary.&lt;directory&gt;.get](./crm-dictionary-get-rest-v3.md) | Returns a directory value by ID ||
|| [crm.dictionary.&lt;directory&gt;.list](./crm-dictionary-list-rest-v3.md) | Returns directory values with filtering, sorting, and pagination ||
|| [crm.dictionary.&lt;directory&gt;.field.list](#field-methods) | Returns the field descriptions of a directory value ||
|| [crm.dictionary.&lt;directory&gt;.field.get](#field-methods) | Returns a directory value field description by name ||
|#

### Directory Field Descriptions {#field-methods}

Each directory has the `crm.dictionary.<directory>.field.list` and `crm.dictionary.<directory>.field.get` methods. They describe the value fields: `id`, `name`, `sort`, and others. The `field.list` method returns field descriptions in `result.items`. The `field.get` method returns the description of a single field in `result.item`; pass the field name in the `name` parameter. A description contains the `filterable`, `sortable`, and `editable` attributes and has the same structure as in the [crm.&lt;type&gt;.field.list](../fields/crm-entity-field-list-rest-v3.md) method.
