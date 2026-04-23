# Overview of CRM

This section contains methods for working with CRM entities in Bitrix24. It covers leads, deals, contacts, companies, Smart Processes, funnels, stages, timelines, documents, requisites, and automation.

For example, you can create a Smart Process, configure its structure, and then interact with its elements through universal CRM methods.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Quick navigation: [all sections and methods](#all-methods)
>
> User documentation: [CRM implementation steps](https://helpdesk.bitrix24.com/open/23477678/)

## Universal CRM Methods

Universal CRM methods operate through `entityTypeId` and cover basic operations with entities: creation, reading, updating, and filtering. They are suitable for leads, deals, contacts, companies, estimates, invoices, and Smart Processes.

Entities are created using the [crm.item.add](./universal/crm-item-add.md) method and updated with [crm.item.update](./universal/crm-item-update.md). A single entity can be retrieved by its ID using [crm.item.get](./universal/crm-item-get.md), while a list can be obtained with [crm.item.list](./universal/crm-item-list.md).

If the operation pertains to only one type of entity—such as the relationships between deals and contacts—use the methods from the relevant section: [crm.deal.*](./deals/index.md), [crm.lead.*](./leads/index.md), [crm.contact.*](./contacts/index.md), [crm.company.*](./companies/index.md), [crm.quote.*](./quote/index.md).

## What is Included in a CRM Card

A CRM card combines the entity's data, the stage of work with it, and the history of interactions.

**Fields.** The card stores the entity's data, the composition of which depends on its type. A list of available fields can be obtained using the [crm.item.fields](./universal/crm-item-fields.md) method. Common fields are described in the article [Fields of Main CRM Entities](./main-entities-fields.md), while custom fields are configured using the [userfieldconfig.add](./universal/userfieldconfig/userfieldconfig-add.md) or [userfieldconfig.update](./universal/userfieldconfig/userfieldconfig-update.md) methods.

**Funnel and Stage.** For deals and Smart Processes, the card shows which funnel the entity is in and at what stage. To work with funnels, you need `categoryId`, which can be retrieved using [crm.category.list](./universal/category/crm-category-list.md). The list of stages with their `ENTITY_ID` codes can be obtained from [crm.status.list](./status/crm-status-list.md).

**Timeline.** The timeline stores the history of interactions with the CRM entity: activities and comments. To add a record to the entity's card, you typically create an activity using the [crm.activity.add](./timeline/activities/activity-base/crm-activity-add.md) method or a comment using the [crm.timeline.comment.add](./timeline/comments/crm-timeline-comment-add.md) method.

**Documents.** Documents in CRM are created based on templates from the document generator. Templates and numerators are added using the [crm.documentgenerator.template.add](./document-generator/templates/crm-document-generator-template-add.md) and [crm.documentgenerator.numerator.add](./document-generator/numerator/crm-document-generator-numerator-add.md) methods. A document is created and linked to a CRM entity using the [crm.documentgenerator.document.add](./document-generator/documents/crm-document-generator-document-add.md) method.

**Automation.** The card can participate in automation scenarios that depend on the entity's state. A custom trigger is registered using the [crm.automation.trigger.add](./automation/triggers/crm-automation-trigger-add.md) method and then executed for the desired element using the [crm.automation.trigger.execute](./automation/triggers/crm-automation-trigger-execute.md) method. Both methods work only in the context of an [application](../../settings/app-installation/index.md).

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

You can embed an application into CRM entity cards and lists. This allows you to use the application without leaving the card or list.

There are two embedding scenarios:

- Use special [embedding locations](../widgets/crm/index.md), such as a tab, a button above the timeline, or a menu item.
- Create a [custom field](../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md) where the application's interface will be loaded.

### CRM Embedding Locations

Replace `XXX` with the entity code: `LEAD`, `DEAL`, `CONTACT`, `COMPANY`, `QUOTE`, `SMART_INVOICE`. For Smart Processes, specify the numeric identifier of the entity type, for example, `CRM_DYNAMIC_183_DETAIL_TAB`.

- [`CRM_XXX_DETAIL_TAB`](../widgets/crm/detail-tab.md) — tab in the detail card

- [`CRM_XXX_DETAIL_ACTIVITY`](../widgets/crm/detail-activity.md) — button above the card's timeline

- [`CRM_XXX_DETAIL_TOOLBAR`](../widgets/crm/detail-toolbar.md) — dropdown menu item in the upper button of the card

- [`CRM_XXX_DOCUMENTGENERATOR_BUTTON`](../widgets/crm/document-generator-button.md) — dropdown menu item for the document generator

- [`CRM_XXX_ACTIVITY_TIMELINE_MENU`](../widgets/crm/activity-timeline-menu.md) — context menu item for an activity in the card

- [`CRM_XXX_LIST_MENU`](../widgets/crm/index.md) — context menu item in the list of entities

- [`CRM_XXX_LIST_TOOLBAR`](../widgets/crm/list-toolbar.md) — dropdown menu item above the list of entities

- [`CRM_XXX_ROBOT_DESIGNER_TOOLBAR`](../widgets/crm/robot-designer-toolbar.md) — dropdown menu item in the upper button of the robot designer

- [`CRM_ANALYTICS_MENU`](../widgets/crm/analytics-menu.md) — item in the left menu of CRM analytics

- [`CRM_ANALYTICS_TOOLBAR`](../widgets/crm/analytics-toolbar.md) — dropdown menu item in the upper button of CRM analytics

- [`CRM_FUNNELS_TOOLBAR`](../widgets/crm/funnels-toolbar.md) — dropdown menu item in sales funnels

{% note tip "Typical use-cases and scenarios" %}

- [Widget embedding mechanism](../widgets/index.md)
- [Embed a widget in a CRM card](../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## Key Identifiers

#|
|| **Identifier** | **Meaning** | **Where Used** | **How to Obtain** ||
|| `entityTypeId` | CRM entity type | Universal methods, funnels, custom fields | For standard entities — [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md), for Smart Processes — [crm.type.list](./universal/user-defined-object-types/crm-type-list.md) ||
|| `id` | CRM entity identifier | Reading, updating, relationships between entities | From the list of entities [crm.item.list](./universal/crm-item-list.md) or after creating an entity [crm.item.add](./universal/crm-item-add.md) ||
|| `categoryId` | Funnel identifier | Deals and Smart Processes — needed when creating and filtering entities | From the list of funnels [crm.category.list](./universal/category/crm-category-list.md) ||
|| `stageId` | Stage identifier | Creating and filtering deal and Smart Process entities | From the list of stages [crm.status.list](./status/crm-status-list.md) with a filter by `ENTITY_ID` ||
|#

## Relationships with Other Entities

**Users.** The person responsible for the CRM entity is stored in the `assignedById` field. User data can be retrieved using the [user.get](../user/user-get.md) or [user.search](../user/user-search.md) methods.

**Tasks.** Tasks are linked to CRM entities through the `UF_CRM_TASK` field. The CRM entity identifier is passed to this field. The relationship is recorded when creating a task using the [tasks.task.add](../tasks/tasks-task-add.md) method, and it can be read using the [tasks.task.get](../tasks/tasks-task-get.md) method.

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
|| [Leads](./leads/index.md) | When working with leads, their cards, and relationships with contacts | [crm.lead.add](./leads/crm-lead-add.md), [crm.lead.update](./leads/crm-lead-update.md), [crm.lead.list](./leads/crm-lead-list.md)

[All methods in the section](./leads/index.md) ||
|| [Contacts](./contacts/index.md) | For working with contacts, their cards, and relationships with companies | [crm.contact.add](./contacts/crm-contact-add.md), [crm.contact.update](./contacts/crm-contact-update.md), [crm.contact.list](./contacts/crm-contact-list.md)

[All methods in the section](./contacts/index.md) ||
|| [Companies](./companies/index.md) | When working with companies, their cards, and relationships with contacts | [crm.company.add](./companies/crm-company-add.md), [crm.company.update](./companies/crm-company-update.md), [crm.company.list](./companies/crm-company-list.md)

[All methods in the section](./companies/index.md) ||
|| [Estimates](./quote/index.md) | For working with estimates and product items | [crm.quote.add](./quote/crm-quote-add.md), [crm.quote.update](./quote/crm-quote-update.md), [crm.quote.list](./quote/crm-quote-list.md)

[All methods in the section](./quote/index.md) ||
|#

### Settings and Directories

#|
|| **Section** | **When to Use** | **Key Methods** ||
|| [Directories](./status/index.md) | For managing system lists in CRM: stages, sources, types | [crm.status.add](./status/crm-status-add.md), [crm.status.update](./status/crm-status-update.md), [crm.status.list](./status/crm-status-list.md)

[All methods in the section](./status/index.md) ||
|| [Currencies](./currency/index.md) | When managing CRM currencies, base currency, and localization | [crm.currency.add](./currency/crm-currency-add.md), [crm.currency.update](./currency/crm-currency-update.md), [crm.currency.list](./currency/crm-currency-list.md)

[All methods in the section](./currency/index.md) ||
|| [Requisites](./requisites/index.md) | For working with requisites, addresses, and banking information in CRM | [crm.requisite.add](./requisites/universal/crm-requisite-add.md), [crm.requisite.update](./requisites/universal/crm-requisite-update.md), [crm.requisite.list](./requisites/universal/crm-requisite-list.md)

[All methods in the section](./requisites/index.md) ||
|#

### Activities and Documents

#|
|| **Section** | **When to Use** | **Key Methods** ||
|| [Timeline and Activities](./timeline/index.md) | For working with activities, comments, calls, and other timeline records | [crm.activity.add](./timeline/activities/activity-base/crm-activity-add.md), [crm.activity.list](./timeline/activities/activity-base/crm-activity-list.md)

[All methods in the section](./timeline/index.md) ||
|| [Call Lists](./call-list/index.md) | When creating call lists and managing their statuses | [crm.calllist.add](./call-list/crm-calllist-add.md), [crm.calllist.list](./call-list/crm-calllist-list.md)

[All methods in the section](./call-list/index.md) ||
|| [Document Generator](./document-generator/index.md) | For generating documents based on templates and managing templates and numerators | [crm.documentgenerator.document.add](./document-generator/documents/crm-document-generator-document-add.md), [crm.documentgenerator.template.list](./document-generator/templates/crm-document-generator-template-list.md)

[All methods in the section](./document-generator/index.md) ||
|#

### Automation and Analytics

#|
|| **Section** | **When to Use** | **Key Methods** ||
|| [CRM Automation](./automation/index.md) | For registering webhook triggers and application triggers | [crm.automation.trigger.add](./automation/triggers/crm-automation-trigger-add.md), [crm.automation.trigger.execute](./automation/triggers/crm-automation-trigger-execute.md)

[All methods in the section](./automation/index.md) ||
|| [Sales Intelligence](./tracking/index.md) | When linking traces to CRM entities and registering analytics sources | [crm.tracking.trace.add](./tracking/crm-tracking-trace-add.md), [crm.tracking.trace.delete](./tracking/crm-tracking-trace-delete.md)

[All methods in the section](./tracking/index.md) ||
|#

### Additional Tools

#|
|| **Section** | **When to Use** | **Key Methods** ||
|| [Finding and Merging Duplicates](./duplicates/index.md) | For finding and merging duplicate CRM records | [crm.duplicate.findbycomm](./duplicates/crm-duplicate-find-by-comm.md)

[All methods in the section](./duplicates/index.md) ||
|| [Digital Workplaces](./automated-solution/index.md) | When creating and configuring digital workplaces for Smart Processes | [crm.automatedsolution.add](./automated-solution/crm-automated-solution-add.md), [crm.automatedsolution.list](./automated-solution/crm-automated-solution-list.md)

[All methods in the section](./automated-solution/index.md) ||
|| [Auxiliary Objects](./auxiliary/index.md) | For working with enumerations, multiple fields, and other auxiliary CRM objects | [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md)

[All methods in the section](./auxiliary/index.md) ||
|| [Outdated CRM Methods](./outdated/index.md) | For maintaining existing integrations on old method branches | [All methods in the section](./outdated/index.md) ||
|#

### Individual Methods

#|
|| **Method** | **Description** ||
|| [crm.settings.mode.get](./crm-settings-mode-get.md) | Returns the current mode of CRM ||
|| [crm.stagehistory.list](./crm-stage-history-list.md) | Returns the history of the entity's movement through stages ||
|#