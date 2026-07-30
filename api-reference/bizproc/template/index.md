# Workflow Templates: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A workflow template is a logical scheme. It implements business logic using actions and operations within the workflow designer.

{% note info "" %}

Methods for adding, updating, and deleting templates work only within the context of an ../../../settings/app-installation/index.md application. Only the application that created a template can update or delete it.

{% endnote %}

> Quick navigation: [All Methods](#all-methods)
>
> User documentation:
> - [How to Create a Sequential Workflow Template](https://helpdesk.bitrix24.com/open/6034961/)
> - [How to Configure Template Parameters](https://helpdesk.bitrix24.com/open/22522520/)

## Add a Workflow Template

The [bizproc.workflow.template.add](./bizproc-workflow-template-add.md) method adds a template to Bitrix24 from a file with the `.bpt` extension. To obtain the file, configure a workflow template and export it.

![Template Export](./_images/export-bp-template.png)

The resulting file can be used as a template in any Bitrix24 instance.

{% note tip "User documentation" %}

- [Workflow Designer](https://helpdesk.bitrix24.com/open/6035031/)
- [Exporting and Importing Workflow Templates](https://helpdesk.bitrix24.com/open/8605753/)

{% endnote %}

## Linking a Template to a Document

Each template is linked to a base object whose data it manages. For example, a template can be linked to CRM deals. In this case, the base object will be the specific deal for which the workflow is launched.

The link to the base object determines the launch context: you cannot launch a process for a lead using a template designed for a deal.

A template is linked to a document via the `DOCUMENT_TYPE` parameter, which is an array consisting of three items:

- module identifier
- object type
- document type

For example, `['crm', 'CCrmDocumentLead', 'LEAD']`.

The values in the array are interconnected. If the first item is `'crm'`, the remaining items must correspond to CRM. It is important to ensure the correctness of these values.

### Possible Values

**Module Identifier.** Specifies the scope of application for the workflow template.

- `crm` — CRM
- `lists` — Common Lists
- `disk` — Bitrix24 Drive

**Object Identifier.** An object within the specified module. For example, in CRM, an object could be a lead or a deal.

CRM
- `CCrmDocumentLead` — leads
- `CCrmDocumentContact` — contacts
- `CCrmDocumentCompany` — companies
- `CCrmDocumentDeal` — deals
- `Bitrix\Crm\Integration\BizProc\Document\Quote` — quotes
- `Bitrix\Crm\Integration\BizProc\Document\SmartInvoice` — invoices
- `Bitrix\Crm\Integration\BizProc\Document\Dynamic` — SPAs

Lists
- `BizprocDocument` — Workflows in Feed
- `Bitrix\Lists\BizprocDocumentLists` — lists in queue groups

Drive
- `Bitrix\Disk\BizProcDocument`

**Document Type.** A binding to a specific document of the specified object.

CRM
- `LEAD` — leads
- `CONTACT` — contacts
- `COMPANY` — companies
- `DEAL` — deals
- `QUOTE` — quotes
- `SMART_INVOICE` — invoices
- `DYNAMIC_XXX` — SPAs, where XXX is the SPA identifier

Common Lists
- `iblock_XXX` — information block, where XXX is the information block identifier

Drive
- `STORAGE_XXX` — drive data store, where XXX is the data store identifier

## Retrieve a List of Templates

To retrieve a list of all portal templates, use the [bizproc.workflow.template.list](./bizproc-workflow-template-list.md) method. To retrieve a list of application templates, specify the `SYSTEM_CODE` field in the `FILTER` parameter and the application symbolic code, for example, `"SYSTEM_CODE": "rest_app_5"`.

## Overview of Methods {#all-methods}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [bizproc.workflow.template.add](./bizproc-workflow-template-add.md) | Add a business process template from a file ||
|| [bizproc.workflow.template.update](./bizproc-workflow-template-update.md) | Update a template ||
|| [bizproc.workflow.template.list](./bizproc-workflow-template-list.md) | Get a list of templates ||
|| [bizproc.workflow.template.delete](./bizproc-workflow-template-delete.md) | Delete a template ||
|#
