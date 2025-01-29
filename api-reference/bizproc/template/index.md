# Business Process Templates: Overview of Methods

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

## DOCUMENT_TYPE

The `DOCUMENT_TYPE` parameter is always an array of three elements. For example, ['crm', 'CCrmDocumentLead', 'LEAD'], etc. The values in these elements are interdependent. In other words, by selecting 'crm' for the first element of the array, the other values that can be used are not arbitrary but specific and must be related to crm. It is essential to ensure their correctness.

#| 
|| **Element** | **Description** ||
|| **Module ID** | Here, the binding area of the business process template is specified. In Bitrix24, business processes are currently used in CRM entities, the activity stream, universal lists, projects, and Bitrix Disk. Therefore, possible values are:

- crm
- livestream
- lists
- tasks
- disk
  
|| 
|| **Object Type / Entity** | The document type represents a specific object within the specified module/entity. For example, if we specified CRM as the module, we can specify CCrmDocumentLead as the document type to bind the business process template to leads. Possible values:

- for crm: CCrmDocumentLead, CCrmDocumentDeal, CCrmDocumentCompany, CCrmDocumentContact
- for livestream ... ??
- for lists: BizprocDocument, ??
- for tasks: Bitrix\Tasks\Integration\Bizproc\Document\Task, ??
- for disk: Bitrix\Disk\BizProcDocument, ??
 ||

|| **Document Type / Specific Object** | After specifying the object type or entity, it is necessary to clarify by binding to a specific document type or even a specific object, if possible in the particular case. For example, when binding the business process template to tasks, it is necessary to specify the identifier of a specific working group (project). Possible values:

- for CCrmDocumentLead: 'LEAD'
- for CCrmDocumentDeal: 'DEAL'
- for CCrmDocumentCompany: 'COMPANY'
- for CCrmDocumentContact: 'CONTACT'
- for BizprocDocument: 'iblock_22' (binding to a specific universal list with id 22)
- for Bitrix\Disk\BizProcDocument: 'STORAGE_490' (binding to a specific file storage with id 490)
- for Bitrix\Tasks\Integration\Bizproc\Document\Task: 'TASK_PROJECT_13' (binding to a specific working group with id 13)
||
|#

{% endnote %}

{% endif %}

#| 
|| **Method** | **Description** ||
|| [bizproc.workflow.template.add](./bizproc-workflow-template-add.md) | Add a business process template from a file ||
|| [bizproc.workflow.template.update](./bizproc-workflow-template-update.md) | Update a template ||
|| [bizproc.workflow.template.list](./bizproc-workflow-template-list.md) | Get a list of templates ||
|| [bizproc.workflow.template.delete](./bizproc-workflow-template-delete.md) | Delete a template ||
|#