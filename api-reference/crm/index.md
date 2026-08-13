# CRM: Overview of Sections and Methods

CRM methods manage the Bitrix24 customer base: leads, deals, contacts, companies, estimates, invoices, and Smart Processes. They create and update entities, move them through funnels and stages, record the history of work in the timeline, generate documents, and launch automation.

For example, you can create a Smart Process, configure its structure, and then interact with its elements through universal CRM methods.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Quick navigation: [all sections and methods](#all-methods)
>
> User documentation: [CRM implementation steps](https://helpdesk.bitrix24.com/open/23477678/)

## How to Get Started

1. Determine the entity type. The numeric `entityTypeId` identifiers of all types, including Smart Processes, are returned by [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md). The settings of the Smart Processes themselves — whether funnels, stages, automation, and other capabilities of the type are enabled — are returned by [crm.type.list](./universal/user-defined-object-types/crm-type-list.md)
2. Retrieve the entity's set of fields with the [crm.item.fields](./universal/crm-item-fields.md) method. For deals and Smart Processes, select the `categoryId` funnel — [crm.category.list](./universal/category/crm-category-list.md) — and the `stageId` stage — [crm.status.list](./status/crm-status-list.md) — in advance
3. Create an entity with the [crm.item.add](./universal/crm-item-add.md) method and update it with [crm.item.update](./universal/crm-item-update.md)
4. Read the data: a single entity by its identifier is returned by [crm.item.get](./universal/crm-item-get.md), and a selection by [crm.item.list](./universal/crm-item-list.md). CRM list methods return up to 50 entities per request, and the next page is selected with the `start` parameter — the details are in the article [Features of List Methods](../../settings/how-to-call-rest-api/list-methods-pecularities.md)

You can subscribe to entity changes with events: they are described in the sections of the entities themselves, for example [deal events](./deals/events/index.md) and [Smart Process item events](./universal/events/index.md).

CRM operates in classic mode with leads or in simple mode without leads. The current mode is returned by [crm.settings.mode.get](./crm-settings-mode-get.md). In simple mode, a deal is created right away, without a preceding lead.

## Universal Methods or Entity Methods

Universal methods [crm.item.*](./universal/index.md) operate through `entityTypeId` and cover the basic operations: creation, reading, updating, and filtering. They are suitable for leads, deals, contacts, companies, estimates, and invoices, and for Smart Processes they are the only way to work with items. The current invoice type is `SMART_INVOICE` with `entityTypeId = 31`.

If the operation pertains to only one type of entity—such as the relationships between deals and contacts—use the methods from the relevant section: [crm.deal.*](./deals/index.md), [crm.lead.*](./leads/index.md), [crm.contact.*](./contacts/index.md), [crm.company.*](./companies/index.md), [crm.quote.*](./quote/index.md).

The universal methods section has its own subtopics: [funnels](./universal/category/index.md), [detail card sections](./universal/item-details-configuration/index.md), [product items](./universal/product-rows/index.md), [invoices](./universal/invoice.md), [payments](./universal/payment/index.md) and [deliveries](./universal/delivery/index.md), [order linking](./universal/order-entity/index.md), [custom fields](./universal/user-defined-fields/index.md) and [their settings](./universal/userfieldconfig/index.md), [Smart Process types](./universal/user-defined-object-types/index.md), [data import](./universal/import/index.md), and [events](./universal/events/index.md).

The old branches of CRM methods are no longer developed. Invoices are replaced by the [universal methods for invoices](./universal/invoice.md), and their stages are managed through the `SMART_INVOICE_STAGE_xx` directory in the [crm.status.*](./status/index.md) methods. Product items are replaced by [crm.item.productrow.*](./universal/product-rows/index.md), deal funnels by [crm.category.*](./universal/category/index.md), and products, catalogs, catalog sections, and units of measurement by the [product catalog](../catalog/index.md) methods.

Field names differ between the two branches of methods: universal methods use `camelCase`, entity methods use `UPPER_CASE`. The deal stage is returned in the `stageId` field by [crm.item.get](./universal/crm-item-get.md) and in the `STAGE_ID` field by [crm.deal.get](./deals/crm-deal-get.md). The name conversion rules are described in the [Universal CRM Methods](./universal/index.md) section.

## What is Included in a CRM Card

A CRM card combines the entity's data, the stage of work with it, and the history of interactions.

**Fields.** The card stores the entity's data, the composition of which depends on its type. A list of available fields can be obtained using the [crm.item.fields](./universal/crm-item-fields.md) method. Common fields are described in the article [Fields of Main CRM Entities](./main-entities-fields.md). Custom fields are configured using the [userfieldconfig.add](./universal/userfieldconfig/userfieldconfig-add.md) or [userfieldconfig.update](./universal/userfieldconfig/userfieldconfig-update.md) methods — they require the `userfieldconfig` scope and the module scope from `moduleId`, which is `crm` for CRM, as well as the "Allow to modify settings" access permission.

**Funnel and Stage.** For deals and Smart Processes, the card shows which funnel the entity is in and at what stage. To work with funnels, you need `categoryId`, which can be retrieved using [crm.category.list](./universal/category/crm-category-list.md). Stages are returned by [crm.status.list](./status/crm-status-list.md) with a filter by the `ENTITY_ID` directory: `DEAL_STAGE` — the stages of the main deal funnel, `DEAL_STAGE_1` — the stages of the funnel with `categoryId = 1`. The stage code is returned in the `STATUS_ID` field: for the main funnel it is `NEW` or `PREPARATION`, and for an additional one it carries the funnel prefix, for example `C1:NEW`. This code is passed in the `stageId` field of universal methods or in `STAGE_ID` of entity methods.

**Timeline.** The timeline stores the history of interactions with the CRM object: activities and comments. To add a record to the entity's card, you typically create a universal activity using the [crm.activity.todo.add](./timeline/activities/todo/crm-activity-todo-add.md) method or a comment using the [crm.timeline.comment.add](./timeline/comments/crm-timeline-comment-add.md) method.

**Documents.** Documents are generated from document generator templates: a template is added with the [crm.documentgenerator.template.add](./document-generator/templates/crm-document-generator-template-add.md) method, and the document itself is created and linked to a CRM object with the [crm.documentgenerator.document.add](./document-generator/documents/crm-document-generator-document-add.md) method.

**Automation.** The card participates in automation scenarios that depend on the entity's state. An application registers its own trigger with the [crm.automation.trigger.add](./automation/triggers/crm-automation-trigger-add.md) method and executes it with the [crm.automation.trigger.execute](./automation/triggers/crm-automation-trigger-execute.md) method — both methods work only in the context of an [application](../../settings/app-installation/index.md).

{% note tip "Typical use-cases and scenarios" %}

- [How to add a custom field to a Smart Process](../../tutorials/crm/how-to-add-crm-objects/how-to-add-user-field-to-spa.md)
- [How to create a new funnel with stages in a Smart Process](../../tutorials/crm/how-to-add-crm-objects/how-to-add-category-to-spa.md)
- [How to add an activity to a contact card](../../tutorials/crm/how-to-add-crm-objects/how-to-add-activity-to-contact.md)
- [How to add a template and create a document based on it](../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)
- [All CRM tutorials](../../tutorials/crm/index.md)

{% endnote %}

## Smart Processes

Smart Processes are custom types of CRM entities for business scenarios that go beyond standard leads, deals, contacts, and companies. They are used to describe contract approvals, internal requests, or equipment accounting.

For a Smart Process, unlike standard entities, the structure is configured first. The type is created using the [crm.type.add](./universal/user-defined-object-types/crm-type-add.md) method, which returns the `entityTypeId` of the new Smart Process. A list of existing types and their `entityTypeId` can be retrieved using [crm.type.list](./universal/user-defined-object-types/crm-type-list.md).

Custom fields are added using the [userfieldconfig.add](./universal/userfieldconfig/userfieldconfig-add.md) method. If necessary, funnels can be configured separately using the [crm.category.add](./universal/category/crm-category-add.md) method and stages using [crm.status.add](./status/crm-status-add.md).

After configuring the structure, you can work with elements using the [crm.item.*](./universal/index.md) methods, just like with standard CRM entities.

{% note tip "User documentation" %}

[Smart Process Automation in CRM](https://helpdesk.bitrix24.com/open/19141012/)

{% endnote %}

## Widgets

An application can be embedded into a CRM entity card or list — as its own tab, as a menu item of a card, a list, or analytics, as a button above the timeline, or in sales funnels. All the placements, supported entity types, and the parameters passed to the handler are collected in the section [Widgets in CRM: Overview of Placements](../widgets/crm/index.md), and the general mechanism is described in the article [Widget Embedding Mechanism](../widgets/index.md).

The placement code follows a pattern like `CRM_XXX_DETAIL_TAB`: replace `XXX` with `LEAD`, `DEAL`, `CONTACT`, `COMPANY`, `QUOTE`, `SMART_INVOICE`, `ORDER`, or `ACTIVITY`, and for Smart Processes use `DYNAMIC_` followed by the numeric identifier of the type, for example `CRM_DYNAMIC_183_DETAIL_TAB`.

The second way to embed an application is a [custom field](../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md) that loads the application's interface. A complete example is walked through in the tutorial [How to Embed a Widget into a CRM Item Tab](../../tutorials/crm/crm-widgets/widget-as-detail-tab.md).

## Key Identifiers

#|
|| **Identifier** | **Meaning** | **Where Used** | **How to Obtain** ||
|| `entityTypeId` | CRM object type | Universal methods, funnels, custom fields | All types, including Smart Processes — [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md); the settings of a Smart Process — [crm.type.list](./universal/user-defined-object-types/crm-type-list.md) ||
|| `id` | CRM object identifier | Reading, updating, relationships between entities | From the list of entities [crm.item.list](./universal/crm-item-list.md) or after creating an entity [crm.item.add](./universal/crm-item-add.md) ||
|| `categoryId` | Funnel identifier | Deals and Smart Processes — needed when creating and filtering entities | From the list of funnels [crm.category.list](./universal/category/crm-category-list.md) ||
|| `stageId` | Stage identifier | Creating and filtering deal and Smart Process entities | From the list of stages [crm.status.list](./status/crm-status-list.md) with a filter by `ENTITY_ID` ||
|#

## Relationships with Other Entities

CRM entities are linked to Bitrix24 users, tasks, the product catalog, and telephony.

**Users.** The person responsible for the CRM object is stored in the `assignedById` field in universal methods and in `ASSIGNED_BY_ID` in entity methods. User data can be retrieved using the [user.get](../user/user-get.md) or [user.search](../user/user-search.md) methods.

**Tasks.** Tasks are linked to CRM entities through the multiple field `UF_CRM_TASK`. It takes an array of identifiers prefixed with the entity type, for example `["D_10", "C_7"]`. The prefixes are listed in the article [Data Types and Structure of Objects](./data-types.md#object_type). The relationship is recorded when creating a task using the [tasks.task.add](../tasks/tasks-task-add.md) method, and it can be read using the [tasks.task.get](../tasks/tasks-task-get.md) method. For the field to accept Smart Process items, enable task linking for the entity type with the `linkedUserFields` parameter in the [crm.type.update](./universal/user-defined-object-types/crm-type-update.md) method.

**Catalog.** Product items in deals and estimates are sourced from the product catalog. Products can be managed using the [catalog.product.*](../catalog/product/index.md) methods.

**Telephony.** Calls create activities in the CRM timeline. The [telephony.externalcall.finish](../telephony/telephony-external-call-finish.md) method ends the call and returns the identifier of the created activity in the `CRM_ACTIVITY_ID` parameter.

## Overview of Sections and Methods {#all-methods}

> Scope: [`crm`](../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Reference Materials

#|
|| **Article** | **Description** ||
|| [Data Types and Structure of Objects in the REST API CRM](./data-types.md) | What is `entityTypeId`, what identifiers exist, and how CRM entities are structured ||
|| [Fields of Main CRM Entities](./main-entities-fields.md) | Fields of key CRM entities in one place ||
|| [Typical Use-Cases and Tutorials](./tutorials.md) | Practical scenarios and examples of using CRM ||
|#

### CRM Entities

#|
|| **Section** | **When to Use** | **Key Methods** ||
|| [Universal CRM Methods](./universal/index.md) | For working with CRM entities and Smart Processes through `entityTypeId` | [crm.item.add](./universal/crm-item-add.md), [crm.item.update](./universal/crm-item-update.md), [crm.item.list](./universal/crm-item-list.md)

[All methods in the section](./universal/index.md) ||
|| [Deals](./deals/index.md) | For working with deals, their cards, and relationships with contacts | [crm.deal.add](./deals/crm-deal-add.md), [crm.deal.update](./deals/crm-deal-update.md), [crm.deal.list](./deals/crm-deal-list.md)

[All methods in the section](./deals/index.md) ||
|| [Leads](./leads/index.md) | For working with leads, their cards, and relationships with contacts | [crm.lead.add](./leads/crm-lead-add.md), [crm.lead.update](./leads/crm-lead-update.md), [crm.lead.list](./leads/crm-lead-list.md)

[All methods in the section](./leads/index.md) ||
|| [Contacts](./contacts/index.md) | For working with contacts, their cards, and relationships with companies | [crm.contact.add](./contacts/crm-contact-add.md), [crm.contact.update](./contacts/crm-contact-update.md), [crm.contact.list](./contacts/crm-contact-list.md)

[All methods in the section](./contacts/index.md) ||
|| [Companies](./companies/index.md) | For working with companies, their cards, and relationships with contacts | [crm.company.add](./companies/crm-company-add.md), [crm.company.update](./companies/crm-company-update.md), [crm.company.list](./companies/crm-company-list.md)

[All methods in the section](./companies/index.md) ||
|| [Estimates](./quote/index.md) | For working with estimates and product items | [crm.quote.add](./quote/crm-quote-add.md), [crm.quote.update](./quote/crm-quote-update.md), [crm.quote.list](./quote/crm-quote-list.md)

[All methods in the section](./quote/index.md) ||
|#

### Settings and Directories

#|
|| **Section** | **When to Use** | **Key Methods** ||
|| [Directories](./status/index.md) | For managing system lists in CRM: stages, sources, types | [crm.status.add](./status/crm-status-add.md), [crm.status.update](./status/crm-status-update.md), [crm.status.list](./status/crm-status-list.md)

[All methods in the section](./status/index.md) ||
|| [Currencies](./currency/index.md) | For managing CRM currencies, base currency, and localization | [crm.currency.add](./currency/crm-currency-add.md), [crm.currency.update](./currency/crm-currency-update.md), [crm.currency.list](./currency/crm-currency-list.md)

[All methods in the section](./currency/index.md) ||
|| [Requisites](./requisites/index.md) | For working with requisites, addresses, and banking information in CRM | [crm.requisite.add](./requisites/universal/crm-requisite-add.md), [crm.requisite.update](./requisites/universal/crm-requisite-update.md), [crm.requisite.list](./requisites/universal/crm-requisite-list.md)

[All methods in the section](./requisites/index.md) ||
|#

### Activities and Documents

#|
|| **Section** | **When to Use** | **Key Methods** ||
|| [Timeline and Activities](./timeline/index.md) | For working with activities, comments, calls, and other timeline records | [crm.activity.todo.add](./timeline/activities/todo/crm-activity-todo-add.md), [crm.timeline.comment.add](./timeline/comments/crm-timeline-comment-add.md)

[All methods in the section](./timeline/index.md) ||
|| [Call Lists](./call-list/index.md) | For creating call lists and managing their statuses | [crm.calllist.add](./call-list/crm-calllist-add.md), [crm.calllist.list](./call-list/crm-calllist-list.md)

[All methods in the section](./call-list/index.md) ||
|| [Document Generator](./document-generator/index.md) | For generating documents based on templates and managing templates and numerators | [crm.documentgenerator.document.add](./document-generator/documents/crm-document-generator-document-add.md), [crm.documentgenerator.template.list](./document-generator/templates/crm-document-generator-template-list.md)

[All methods in the section](./document-generator/index.md) ||
|#

### Automation and Analytics

#|
|| **Section** | **When to Use** | **Key Methods** ||
|| [CRM Automation](./automation/index.md) | For executing configured webhook triggers and registering application triggers | [crm.automation.trigger](./automation/crm-automation-trigger.md), [crm.automation.trigger.add](./automation/triggers/crm-automation-trigger-add.md), [crm.automation.trigger.execute](./automation/triggers/crm-automation-trigger-execute.md)

[All methods in the section](./automation/index.md) ||
|| [Sales Intelligence](./tracking/index.md) | For creating traces and linking CRM entities to lead sources | [crm.tracking.trace.add](./tracking/crm-tracking-trace-add.md), [crm.tracking.trace.delete](./tracking/crm-tracking-trace-delete.md)

[All methods in the section](./tracking/index.md) ||
|#

### Additional Tools

#|
|| **Section** | **When to Use** | **Key Methods** ||
|| [Finding and Merging Duplicates](./duplicates/index.md) | For finding and merging duplicate CRM records | [crm.duplicate.findbycomm](./duplicates/crm-duplicate-find-by-comm.md), [crm.entity.mergeBatch](./duplicates/crm-entity-merge-batch.md)

[All methods in the section](./duplicates/index.md) ||
|| [Digital Workplaces](./automated-solution/index.md) | For creating and configuring digital workplaces for Smart Processes | [crm.automatedsolution.add](./automated-solution/crm-automated-solution-add.md), [crm.automatedsolution.list](./automated-solution/crm-automated-solution-list.md)

[All methods in the section](./automated-solution/index.md) ||
|| [Auxiliary Objects](./auxiliary/index.md) | For working with enumerations, multiple fields, and other auxiliary CRM objects | [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md)

[All methods in the section](./auxiliary/index.md) ||
|#

### Individual Methods

#|
|| **Method** | **Description** ||
|| [crm.settings.mode.get](./crm-settings-mode-get.md) | Returns the current mode of CRM ||
|| [crm.stagehistory.list](./crm-stage-history-list.md) | Returns the history of the entity's movement through stages ||
|#