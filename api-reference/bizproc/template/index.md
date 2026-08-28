# Workflow Templates: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A workflow template defines automation logic through actions and operations in the workflow designer. The methods let you add a template from a `.bpt` file, update its parameters, retrieve the list of templates, and delete a template created by the application.

{% note info "" %}

Methods for adding, updating, and deleting templates work only within the context of an [application](../../../settings/app-installation/index.md). Only the application that created a template can update or delete it.

{% endnote %}

> Quick navigation: [All Methods](#all-methods)
>
> User documentation:
> - [How to Create a Sequential Workflow Template](https://helpdesk.bitrix24.com/open/6034961/)
> - [How to Configure Template Parameters](https://helpdesk.bitrix24.com/open/22522520/)

## How to Get Started

1. Add a template using the [bizproc.workflow.template.add](./bizproc-workflow-template-add.md) method
2. Retrieve the list of templates using the [bizproc.workflow.template.list](./bizproc-workflow-template-list.md) method
3. Update the template using the [bizproc.workflow.template.update](./bizproc-workflow-template-update.md) method
4. Delete an outdated template using the [bizproc.workflow.template.delete](./bizproc-workflow-template-delete.md) method

## How to Prepare a Template File

The [bizproc.workflow.template.add](./bizproc-workflow-template-add.md) method adds a template from a file with the `.bpt` extension. To get the file, configure the workflow template in the designer and export it.

![Template Export](./_images/export-bp-template.png)

The resulting file can be used as a template in any Bitrix24 instance.

{% note tip "User documentation" %}

- [Workflow Designer](https://helpdesk.bitrix24.com/open/6035031/)
- [Exporting and Importing Workflow Templates](https://helpdesk.bitrix24.com/open/8605753/)

{% endnote %}

## Document Type Identifier

`DOCUMENT_TYPE` is specified in the parameters of the [bizproc.workflow.template.add](./bizproc-workflow-template-add.md) method when the application adds a template from a file. In the [bizproc.workflow.template.list](./bizproc-workflow-template-list.md) method, the `MODULE_ID`, `ENTITY`, and `DOCUMENT_TYPE` values are returned in the template fields and can be used for filtering.

`DOCUMENT_TYPE` is an array of three strings. It links the template to the document type for which the workflow will run:

- module identifier, for example `crm`
- object identifier, for example `CCrmDocumentDeal`
- document type, for example `DEAL`

The values in the array are interconnected: if the first item belongs to CRM, the remaining items must also describe a CRM object.

### Possible Values

#|
|| **Module** | **Object Identifier** | **Document Type** | **Description** ||
|| `crm` | `CCrmDocumentLead` | `LEAD` | Leads ||
|| `crm` | `CCrmDocumentContact` | `CONTACT` | Contacts ||
|| `crm` | `CCrmDocumentCompany` | `COMPANY` | Companies ||
|| `crm` | `CCrmDocumentDeal` | `DEAL` | Deals ||
|| `crm` | `Bitrix\Crm\Integration\BizProc\Document\Quote` | `QUOTE` | Quotes ||
|| `crm` | `Bitrix\Crm\Integration\BizProc\Document\SmartInvoice` | `SMART_INVOICE` | Invoices ||
|| `crm` | `Bitrix\Crm\Integration\BizProc\Document\Dynamic` | `DYNAMIC_XXX` | SPAs, where XXX is the SPA identifier ||
|| `lists` | `BizprocDocument` | `iblock_XXX` | Processes in the news feed, where XXX is the information block identifier ||
|| `lists` | `Bitrix\Lists\BizprocDocumentLists` | `iblock_XXX` | Lists in groups, where XXX is the information block identifier ||
|| `disk` | `Bitrix\Disk\BizProcDocument` | `STORAGE_XXX` | Drive storage, where XXX is the storage identifier ||
|#

## What to Consider

- The [bizproc.workflow.template.add](./bizproc-workflow-template-add.md), [bizproc.workflow.template.update](./bizproc-workflow-template-update.md), and [bizproc.workflow.template.delete](./bizproc-workflow-template-delete.md) methods work only in the context of an installed application
- You can update or delete only a template created by the same application
- The document type is set by the `DOCUMENT_TYPE` parameter and determines for which objects the workflow can be launched
- To get the application's templates, pass the `SYSTEM_CODE` field to the filter of the [bizproc.workflow.template.list](./bizproc-workflow-template-list.md) method, for example `"SYSTEM_CODE": "rest_app_5"`

## Relationship with Other Objects

**CRM.** A template can be linked to leads, contacts, companies, deals, quotes, invoices, and SPAs. The relation is defined through `DOCUMENT_TYPE`, for example `['crm', 'CCrmDocumentDeal', 'DEAL']`.

The relation to the base object determines the launch context: you cannot launch a process for a lead using a template designed for a deal.

**Common Lists.** A template can be linked to processes in the news feed or lists in groups. In `DOCUMENT_TYPE`, specify the `lists` module, the object type, and the information block identifier in the `iblock_XXX` format.

**Drive.** A template can be linked to Drive storage. In `DOCUMENT_TYPE`, specify the `disk` module, the `Bitrix\Disk\BizProcDocument` object, and the storage identifier in the `STORAGE_XXX` format.

## Overview of Methods {#all-methods}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [bizproc.workflow.template.add](./bizproc-workflow-template-add.md) | Adds a business process template from a file ||
|| [bizproc.workflow.template.update](./bizproc-workflow-template-update.md) | Updates a template ||
|| [bizproc.workflow.template.list](./bizproc-workflow-template-list.md) | Retrieves the list of templates ||
|| [bizproc.workflow.template.delete](./bizproc-workflow-template-delete.md) | Deletes a template ||
|#
