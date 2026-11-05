# CRM: Overview of Sections and Methods

CRM methods manage the Bitrix24 customer base: leads, deals, contacts, companies, estimates, invoices, and SPAs. They create and update items, move them through pipelines and stages, record the history of work in the timeline, generate documents, and launch automation.

CRM methods in REST 3.0 are intended for new development, and they are called at the `/rest/api/` address. The methods of the [previous API version](./previous-api.md) are called at the `/rest/` address and are kept to support existing integrations.

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Quick navigation: [all sections and methods](#all-methods)
>
> User documentation: [CRM implementation steps](https://helpdesk.bitrix24.com/open/23477678/)

## Which API Version to Choose {#choose}

#|
|| **If you need to** | **Open the section** ||
|| Start a new integration: work with CRM and SPA items, their fields, pipelines and stages, products, requisites, addresses, activities, and card views | [REST 3.0 Methods](#method-groups) ||
|| Collect requests from a website and create CRM objects from them | [CRM Forms](./form/index.md) ||
|| Configure SPA types, work with the timeline, documents, automation, and other sections that are not available in REST 3.0 | [Sections Available Only in the Previous Version](#no-rest-v3) ||
|| Support an existing integration built on methods at the `/rest/` address | [Previous API Version](./previous-api.md) ||
|| Migrate an integration from the methods of the previous version to REST 3.0 | [Migrating to the New CRM Methods](./migration.md) ||
|#

## Object Type in the Method Name {#method-name}

In REST 3.0, the object type is part of the method name: `crm.<type>.<action>`. For example, `crm.deal.add` creates a deal, and `crm.smartprocess1038.add` creates an SPA item with the type ID `1038`. That is why the documentation has one page per action for all types, and each page starts with a table that lists the full method name for every type.

#|
|| **Object type** | **Segment in the method name** | **Example** ||
|| Deal | `deal` | `crm.deal.list` ||
|| Lead | `lead` | `crm.lead.list` ||
|| Contact | `contact` | `crm.contact.list` ||
|| Company | `company` | `crm.company.list` ||
|| Estimate | `quote` | `crm.quote.list` ||
|| Invoice | `smartinvoice` | `crm.smartinvoice.list` ||
|| My company | `mycompany` | `crm.mycompany.list` ||
|| CRM document | `smartdocument` | `crm.smartdocument.productrow.list` ||
|| B2E document | `smartb2edocument` | `crm.smartb2edocument.detailpagelayout.list` ||
|| SPA | `smartprocess<entityTypeId>` | `crm.smartprocess1038.list` ||
|#

The SPA type ID `entityTypeId` is returned by the [crm.type.list](./universal/user-defined-object-types/crm-type-list.md) method. REST 3.0 has no equivalent of this method, so call it at the previous version address `/rest/`.

The method name is written in lowercase: `crm.smartinvoice.add` and `crm.deal.productrow.list`, not `crm.smartInvoice.add` and `crm.deal.productRow.list`. A method with different casing or with a type that does not exist, for example `crm.smartprocess99999.add`, returns the `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION` error with status 404.

## Methods Available for Each Object Type {#method-groups}

Not every group of REST 3.0 methods is available for every type. For example, only deals and SPAs have stages, and only contacts and companies have requisites.

#|
|| **Method group** | **deal** | **lead** | **contact** | **company** | **quote** | **smartinvoice** | **mycompany** | **smartdocument** | **smartb2edocument** | **smartprocess** ||
|| [Items](./rest-v3/items/index.md) | + | + | + | + | + | + | + | — | — | + ||
|| Primary contact `setprimarycontact` | + | + | — | — | + | + | — | + | + | + ||
|| [Field descriptions](./rest-v3/fields/index.md) `field.get`, `field.list` | + | + | + | + | + | + | + | — | — | + ||
|| [Custom fields and required fields](./rest-v3/fields/index.md) | + | + | + | + | + | + | — | — | — | + ||
|| [Pipelines](./rest-v3/categories/index.md) | + | — | + | + | — | — | — | — | — | + ||
|| [Stages](./rest-v3/categories/index.md) | + | — | — | — | — | — | — | — | — | + ||
|| [Product items](./rest-v3/product-rows/index.md) | + | + | — | — | + | + | — | + | — | + ||
|| [Requisites and bank details](./rest-v3/requisites/index.md) | — | — | + | + | — | — | — | — | — | — ||
|| [Client requisites](./rest-v3/requisites/index.md) | + | — | — | — | + | + | — | + | — | + ||
|| [My company requisites](./rest-v3/requisites/index.md) | + | — | — | — | — | + | — | — | — | — ||
|| [Addresses](./rest-v3/addresses/index.md) | — | + | + | + | — | — | — | — | — | — ||
|| [Card views](./rest-v3/detail-page-layout/index.md) | + | + | + | + | + | + | — | + | + | + ||
|| [Activities](./rest-v3/activities/index.md) | + | + | + | + | + | + | — | + | — | + ||
|#

[Directories](./rest-v3/dictionaries/index.md) and [lead mode](./rest-v3/lead-mode/index.md) are not tied to an object type: `crm.dictionary.<directory>.*`, `crm.lead.isenabled`, `crm.lead.enable`, `crm.lead.disable`.

The exact list of methods available in your Bitrix24 is returned by the [OpenAPI documentation](../rest-v3.md#openapi).

### SPA Methods Depend on the Type Settings

For an SPA, a "+" in the table means that the group is available when the corresponding type setting is enabled:

- without pipelines `isCategoriesEnabled`, there are no pipeline or stage methods
- without stages `isStagesEnabled`, there are no stage methods
- without products `isLinkWithProductsEnabled`, there are no product item methods
- without client linking `isClientEnabled`, there is no `setprimarycontact` method

The set of fields also depends on the settings. The fields that disappear without each setting are described on the [SPA fields](./rest-v3/items/fields-smart-process.md) page.

## How to Get Started

1. Determine the object type and the method name from the [Object Type in the Method Name](#method-name) table. For an SPA, retrieve `entityTypeId` with the [crm.type.list](./universal/user-defined-object-types/crm-type-list.md) method
2. Look up the fields of the type on its fields page, for example [Deal fields](./rest-v3/items/fields-deal.md), or with the [crm.&lt;type&gt;.field.list](./rest-v3/fields/crm-entity-field-list-rest-v3.md) method
3. Create an item with the [crm.&lt;type&gt;.add](./rest-v3/items/crm-entity-add-rest-v3.md) method
4. Retrieve items with the [crm.&lt;type&gt;.list](./rest-v3/items/crm-entity-list-rest-v3.md) method. Without the `select` parameter, the `get` and `list` methods return only `id`, so list the fields you need in `select`
5. Add products, requisites, or addresses to the item with the methods of the corresponding group from the [overview of methods](#rest-v3)

How REST 3.0 differs from the previous version and how to migrate an existing integration to it is described on the [Migrating to the New CRM Methods](./migration.md) page.

{% note tip "Typical use-cases and scenarios" %}

- [How to add a custom field to a Smart Process](../../tutorials/field-types/how-to-add-user-field-to-spa.md)
- [How to create a new funnel with stages in a Smart Process](../../tutorials/crm/how-to-add-crm-objects/how-to-add-category-to-spa.md)
- [How to add an activity to a contact card](../../tutorials/crm/how-to-add-crm-objects/how-to-add-activity-to-contact.md)
- [How to add a template and create a document based on it](../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)
- [All CRM tutorials](../../tutorials/crm/index.md)

{% endnote %}

## Widgets

An application can be embedded into a CRM entity card or list — as its own tab, as a menu item of a card, a list, or analytics, as a button above the timeline, or in sales funnels. All the placements and the parameters passed to the handler are collected in the section [Widgets in CRM: Overview of Placements](../widgets/crm/index.md).

## Relationships with Other Entities

CRM entities are linked to Bitrix24 users, tasks, the product catalog, telephony, and CRM forms.

**Users.** The person responsible for the CRM object is stored in the `assignedById` field in REST 3.0 and universal methods and in the `ASSIGNED_BY_ID` field in entity methods. User data can be retrieved using the [user.get](../user/user-get.md) or [user.search](../user/user-search.md) methods.

**Tasks.** Tasks are linked to CRM entities through the multiple field `UF_CRM_TASK`. It takes an array of identifiers prefixed with the entity type, for example `["D_10", "C_7"]`. The prefixes are listed in the article [Data Types and Structure of Objects](./data-types.md#object_type). The relationship is recorded when creating a task using the [tasks.task.add](../tasks/tasks-task-add.md) method, and it can be read using the [tasks.task.get](../tasks/tasks-task-get.md) method. For the field to accept Smart Process items, enable task linking for the entity type with the `linkedUserFields` parameter in the [crm.type.update](./universal/user-defined-object-types/crm-type-update.md) method.

**Catalog.** Product items in deals and estimates are sourced from the product catalog. Products can be managed using the [catalog.product.*](../catalog/product/index.md) methods.

**Telephony.** Calls create activities in the CRM timeline. The [telephony.externalcall.finish](../telephony/telephony-external-call-finish.md) method ends the call and returns the identifier of the created activity in the `CRM_ACTIVITY_ID` parameter.

**CRM forms.** Submitting a CRM form creates a lead, a deal, an estimate, or an SPA item. Forms are created and configured with the REST 3.0 methods [crm.form.*](./form/index.md).

## Overview of Sections and Methods {#all-methods}

> Scope: [`crm`](../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### REST 3.0 Methods {#rest-v3}

#|
|| **Section** | **What the methods do** ||
|| [Items](./rest-v3/items/index.md) | Create, update, retrieve, count, and delete CRM items, work with multiple fields and the primary contact. This section also has the fields page for each object type ||
|| [Field Management](./rest-v3/fields/index.md) | Return field descriptions, create custom fields and list values, and set required fields ||
|| [Pipelines and Stages](./rest-v3/categories/index.md) | Create and configure pipelines and stages of deals and SPAs, and pipelines of contacts and companies ||
|| [Product Items](./rest-v3/product-rows/index.md) | Add, update, and replace products in deals, leads, estimates, invoices, documents, and SPAs ||
|| [Requisites](./rest-v3/requisites/index.md) | Work with requisites, bank details, and requisite addresses of contacts and companies, and link client and my company requisites to items ||
|| [Addresses](./rest-v3/addresses/index.md) | Work with addresses of contacts, companies, and leads ||
|| [Card Views](./rest-v3/detail-page-layout/index.md) | Configure the sections and fields of the item card, select the view for a user, and set the common view ||
|| [Activities](./rest-v3/activities/index.md) | Retrieve, complete, and delete activities, manage activity links to items, and work with emails. Activities and comments are created by the [timeline](./timeline/index.md) methods of the previous version ||
|| [Directories](./rest-v3/dictionaries/index.md) | Return and modify CRM directories: deal and contact types, sources, salutations, industries, and others ||
|| [Lead Mode](./rest-v3/lead-mode/index.md) | Check, enable, and disable leads in CRM ||
|| [CRM Forms](./form/index.md) | Create, configure, enable, and delete CRM forms, and retrieve the code for embedding a form on a website ||
|#

### Sections Not Available in REST 3.0 {#no-rest-v3}

These methods are called at the previous version address `/rest/`.

#|
|| **Section** | **What the methods do** ||
|| [SPA Types](./universal/user-defined-object-types/index.md) | Create and configure SPA types. The type ID `entityTypeId` is required for the REST 3.0 method name, for example `crm.smartprocess1038.add` ||
|| [Timeline and Activities](./timeline/index.md) | Create activities, comments, log entries, and other timeline items ||
|| [Document Generator](./document-generator/index.md) | Generate documents from templates and manage templates and numerators ||
|| [CRM Automation](./automation/index.md) | Run webhook triggers and register application triggers ||
|| [Digital Workplaces](./automated-solution/index.md) | Create and configure digital workplaces for SPAs ||
|| [Currencies](./currency/index.md) | Manage CRM currencies, the base currency, and localization ||
|| [Call Lists](./call-list/index.md) | Create call lists and manage their statuses ||
|| [Finding and Merging Duplicates](./duplicates/index.md) | Find and merge duplicate CRM records ||
|| [Sales Intelligence](./tracking/index.md) | Create traces and link CRM objects to lead sources ||
|| [Stage Movement History](./crm-stage-history-list.md) | Returns the history of item movement through stages ||
|#

### Previous API Version

The universal methods `crm.item.*`, the methods of deals, leads, contacts, companies, and estimates, requisites, the `crm.status.*` directories, and auxiliary objects are kept to support existing integrations. They are collected in the [Previous API Version](./previous-api.md) section.
