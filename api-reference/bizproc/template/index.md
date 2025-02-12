# Business Process Templates: Overview of Methods

A business process template is a logical scheme. It implements business logic through actions and operations in the business process designer.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: 
> - [How to create a sequential business process template](https://helpdesk.bitrix24.com/open/6034961/)
> - [How to configure template parameters](https://helpdesk.bitrix24.com/open/22522520/)

## Add a Business Process Template

The method [bizproc.workflow.template.add](./bizproc-workflow-template-add.md) adds a template to Bitrix24 from a file with the `.bpt` extension. To obtain the file, configure the business process template and export it.

The resulting file can be used as a template in the desired Bitrix24.

{% note tip "User Documentation" %}

-  [Business Process Designer](https://helpdesk.bitrix24.com/open/6035031/)
-  [Exporting and Importing Business Process Templates](https://helpdesk.bitrix24.com/open/8605753/)

{% endnote %}

## Application Context

The system ties the new template to an [application](../../app-installation/index.md). Templates created by the method [bizproc.workflow.template.add](./bizproc-workflow-template-add.md) can only be updated or deleted within the context of the application they were created in.

## Template Connection to Document

Each template is linked to a base object whose data it manages. For example, a template may be linked to CRM deals. In this case, the base object will be a specific deal for which the business process is initiated.

The connection to the base object defines the context of execution: a process cannot be initiated for a lead using a template meant for a deal.

The template is linked to the document through the `DOCUMENT_TYPE` parameter, which is an array of three elements:

-  module identifier
-  object type
-  document type

For example, `['crm', 'CCrmDocumentLead', 'LEAD']`.

The values in the array are interrelated. If the first element is `'crm'`, the others must correspond to CRM. It is important to ensure the correctness of the values.

### Possible Values

**Module Identifier.** Indicates the scope of the business process template.

-  `crm` — CRM
-  `lists` — Universal Lists
-  `disk` — Bitrix24 Disk

**Object Identifier.** The object within the specified module. For example, in CRM, the object can be a lead or a deal.

CRM
-  `CCrmDocumentLead` — leads
-  `CCrmDocumentContact` — contacts
-  `CCrmDocumentCompany` — companies
-  `CCrmDocumentDeal` — deals
-  `Bitrix\Crm\Integration\BizProc\Document\Quote` — estimates
-  `Bitrix\Crm\Integration\BizProc\Document\SmartInvoice` — invoices
-  `Bitrix\Crm\Integration\BizProc\Document\Dynamic` — SPAs

Lists
-  `BizprocDocument` — processes in the news feed
-  `Bitrix\Lists\BizprocDocumentLists` — lists in groups

Disk
-  `Bitrix\Disk\BizProcDocument`

**Document Type.** Binding to a specific document of the specified object.

CRM
-  `LEAD` — leads
-  `CONTACT` — contacts
-  `COMPANY` — companies
-  `DEAL` — deals
-  `QUOTE` — estimates
-  `SMART_INVOICE` — invoices
-  `DYNAMIC_XXX` — SPAs, where XXX is the SPA identifier

Universal Lists
-  `iblock_XXX` — information block, where XXX is the information block identifier

Disk
-  `STORAGE_XXX` — disk storage, where XXX is the storage identifier

## Get List of Templates

To retrieve a list of all templates in the account, use the method [bizproc.workflow.template.list](./bizproc-workflow-template-list.html). To get a list of application templates, specify the `FILTER` parameter with the `SYSTEM_CODE` field and the symbolic code of the application, for example, `"SYSTEM_CODE": "rest_app_5"`.

## Overview of Methods {#all-methods}

#| 
|| **Method** | **Description** ||
|| [bizproc.workflow.template.add](./bizproc-workflow-template-add.md) | Add a business process template from a file ||
|| [bizproc.workflow.template.update](./bizproc-workflow-template-update.md) | Update a template ||
|| [bizproc.workflow.template.list](./bizproc-workflow-template-list.md) | Get a list of templates ||
|| [bizproc.workflow.template.delete](./bizproc-workflow-template-delete.md) | Delete a template || 
|#