# CRM Methods of the Previous API Version: Overview

The methods of the previous API version are called at the `/rest/` address and are kept to support existing integrations. CRM methods in REST 3.0 are intended for new development, and they are described in the [CRM overview](./index.md). You can call methods of both versions from the same integration.

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Quick navigation: [all sections and methods](#all-methods)

## Universal Methods and Object Methods

In the previous version, CRM items are handled by two sets of methods:

- the universal methods [crm.item.*](./universal/index.md) — the same methods for all types, with the object type passed in the `entityTypeId` parameter. Field names are written in `camelCase`, for example `assignedById`
- the object methods `crm.deal.*`, `crm.lead.*`, `crm.contact.*`, `crm.company.*`, `crm.quote.*` — a separate set for each type. Field names are written in uppercase, for example `ASSIGNED_BY_ID`

SPA types are created and configured with the [crm.type.*](./universal/user-defined-object-types/index.md) methods. REST 3.0 has no equivalents of these methods, so integrations built on REST 3.0 need them as well.

The REST 3.0 equivalents of the methods, their limitations, and the differences between the versions are described on the [Migrating to the New CRM Methods](./migration.md) page.

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
|#

### CRM Objects

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
|| [Requisites](./requisites/index.md) | For working with requisites, addresses, and banking information in CRM | [crm.requisite.add](./requisites/universal/crm-requisite-add.md), [crm.requisite.update](./requisites/universal/crm-requisite-update.md), [crm.requisite.list](./requisites/universal/crm-requisite-list.md)

[All methods in the section](./requisites/index.md) ||
|| [Directories](./status/index.md) | For managing system lists in CRM: stages, sources, types | [crm.status.add](./status/crm-status-add.md), [crm.status.update](./status/crm-status-update.md), [crm.status.list](./status/crm-status-list.md)

[All methods in the section](./status/index.md) ||
|| [Auxiliary Objects](./auxiliary/index.md) | For working with enumerations, multiple fields, and other auxiliary CRM objects | [crm.enum.ownertype](./auxiliary/enum/crm-enum-owner-type.md)

[All methods in the section](./auxiliary/index.md) ||
|#

### Individual Methods

#|
|| **Method** | **Description** ||
|| [crm.settings.mode.get](./crm-settings-mode-get.md) | Returns the current mode of CRM ||
|#
