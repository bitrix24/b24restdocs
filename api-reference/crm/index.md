# CRM: Overview of Sections and Methods

CRM methods manage the Bitrix24 customer base: leads, deals, contacts, companies, estimates, invoices, and Smart Processes. They create and update entities, move them through funnels and stages, record the history of work in the timeline, generate documents, and launch automation.

CRM methods are available in two API versions. [CRM methods in REST 3.0](./rest-v3/index.md) are intended for new development, and the methods of the previous version are kept for existing integrations.

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Quick navigation: [all sections and methods](#all-methods)
>
> User documentation: [CRM implementation steps](https://helpdesk.bitrix24.com/open/23477678/)

## Which API Version to Choose {#choose}

[CRM in REST 3.0](./rest-v3/index.md) is the main section for new development. The methods are called at the `/rest/api/` address, and their names are the same as in the previous version: `crm.deal.add` creates a deal in both versions, and the version is determined by the call address.

The methods of the previous version are called at the `/rest/` address. The universal methods [crm.item.*](./universal/index.md) and the entity methods `crm.deal.*`, `crm.lead.*`, `crm.contact.*`, and others are kept to support existing integrations. Some sections are still available only in the previous version. You can call methods of both versions from the same integration.

#|
|| **If you need to** | **Open the section** ||
|| Work with CRM and Smart Process items, their fields, funnels and stages, products, requisites, addresses, activities, and card views | [CRM in REST 3.0](./rest-v3/index.md) ||
|| Collect requests from a website and create CRM objects from them | [CRM Forms](./form/index.md) ||
|| Create or configure a Smart Process type | [Smart Process types](./universal/user-defined-object-types/index.md), previous version only ||
|| Add an activity or a comment to the timeline | [Timeline and Activities](./timeline/index.md), previous version only ||
|| Generate a document from a template | [Document Generator](./document-generator/index.md), previous version only ||
|| Run automation or a trigger | [CRM Automation](./automation/index.md), previous version only ||
|| Support an existing integration built on methods at the `/rest/` address | [Universal CRM Methods](./universal/index.md) and the entity sections in the [overview of sections and methods](#all-methods) ||
|#

Currencies, call lists, duplicate search, sales intelligence, and digital workplaces are also still available only in the previous version.

## How to Get Started

1. Determine the entity type and the method name. In REST 3.0, the type is part of the name: `crm.deal.add`, `crm.smartprocess1038.add`. The segments of all types are listed in the [CRM in REST 3.0](./rest-v3/index.md#method-name) overview
2. Look up the fields of the type on its fields page, for example [Deal fields](./rest-v3/items/fields-deal.md), or with the [crm.&lt;type&gt;.field.list](./rest-v3/fields/crm-entity-field-list-rest-v3.md) method
3. Create an item with the [crm.&lt;type&gt;.add](./rest-v3/items/crm-entity-add-rest-v3.md) method
4. Retrieve items with the [crm.&lt;type&gt;.list](./rest-v3/items/crm-entity-list-rest-v3.md) method. Without the `select` parameter, the method returns only `id`, so list the fields you need in `select`

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

**CRM forms.** Submitting a CRM form creates CRM objects according to the scheme in the `crm.entityScheme` field: a lead, a deal with a contact or company, an estimate, or a Smart Process item. Scheme identifiers, deal pipelines, and Smart Processes are returned by [crm.form.configuration.get](./form/setup/crm-form-configuration-get.md), and the codes of CRM object fields for the form are returned by [crm.form.entityfield.list](./form/setup/crm-form-entityfield-list.md). Forms are created and configured with the [crm.form.*](./form/index.md) methods. These are REST 3.0 methods: they are called at an address containing `/rest/api/`, see the [REST 3.0 overview](../rest-v3.md) for details.

## Overview of Sections and Methods {#all-methods}

> Scope: [`crm`](../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Reference Materials

#|
|| **Article** | **Description** ||
|| [Data Types and Structure of Objects in the REST API CRM](./data-types.md) | What is `entityTypeId`, what identifiers exist, and how CRM entities are structured ||
|| [Fields of Main CRM Entities](./main-entities-fields.md) | Fields of key CRM entities in one place ||
|| [CRM Field Length Limits](./field-length-limits.md) | Maximum length of standard CRM fields and how to get custom field limits ||
|| [Typical Use-Cases and Tutorials](./tutorials.md) | Practical scenarios and examples of using CRM ||
|#

### CRM in REST 3.0

#|
|| **Section** | **When to Use** | **Key Methods** ||
|| [CRM in REST 3.0](./rest-v3/index.md) | For new development: items of all object types, their fields, funnels and stages, product items, requisites, addresses, activities, card views, and directories | [crm.&lt;type&gt;.add](./rest-v3/items/crm-entity-add-rest-v3.md), [crm.&lt;type&gt;.update](./rest-v3/items/crm-entity-update-rest-v3.md), [crm.&lt;type&gt;.list](./rest-v3/items/crm-entity-list-rest-v3.md)

[All sections and methods](./rest-v3/index.md#all-methods) ||
|#

### CRM Entities in the Previous API Version

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
|| [CRM Forms](./form/index.md) | For creating forms that collect website requests and create CRM objects, and for retrieving the form embed code | [crm.form.list](./form/crm-form-list.md), [crm.form.add](./form/crm-form-add.md), [crm.form.embedding.get](./form/placement/crm-form-embedding-get.md)

[All methods in the section](./form/index.md) ||
|| [Auxiliary Objects](./auxiliary/index.md) | For working with enumerations, multiple fields, and other auxiliary CRM objects | [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md)

[All methods in the section](./auxiliary/index.md) ||
|#

### Individual Methods

#|
|| **Method** | **Description** ||
|| [crm.settings.mode.get](./crm-settings-mode-get.md) | Returns the current mode of CRM ||
|| [crm.stagehistory.list](./crm-stage-history-list.md) | Returns the history of the entity's movement through stages ||
|#
