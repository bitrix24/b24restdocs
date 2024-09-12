# Starting a Workflow

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed to meet writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent
- links to pages that have not yet been created are missing
- it is completely unclear what lists.element.add has to do with this and what "will be **bistrix_processe**" means. It's just incomprehensible :(
- more detailed descriptions of parameters are needed. What can be a document identifier, for example? Details about PARAMETERS are required.

{% endnote %}

{% endif %}

{% note info "bizproc.workflow.start" %}

{% include notitle [Scope bizproc all](./_includes/scope-bizproc-all.md) %}

{% endnote %}

This method starts a workflow.

To start a workflow from the news feed, use the method [lists.element.add](.). In this case, `IBLOCK_TYPE_ID` will be **bistrix_processe**.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **TEMPLATE_ID** | Identifier of the workflow template. ||
|| **DOCUMENT_ID** | Identifier of the workflow document. ||
|| **PARAMETERS** | Values of the workflow parameters (if the template has parameters). ||
|#

## Examples

```js
function startWf(leadId, tplId, cb)
{
	if (!leadId)
	{
		alert('Lead not selected');
		return;
	}
	var params = {
		TEMPLATE_ID: tplId,
		DOCUMENT_ID: ['crm', 'CCrmDocumentLead', leadId],
		PARAMETERS: null
	};
	BX24.callMethod(
		'bizproc.workflow.start',
		params,
		function(result)
		{
			if(result.error())
			alert("Error: " + result.error());
			else if (cb)
				cb();
		}
	);
}
```

Examples of substitutions for the DOCUMENT_ID parameter:

```php
['crm', 'CCrmDocumentLead', 'LEAD_777'] – Lead
['crm', 'CCrmDocumentCompany', 'COMPANY_777'] – Company
['crm', 'CCrmDocumentContact', 'CONTACT_777'] – Contact
['crm', 'CCrmDocumentDeal', 'DEAL_777'] – Deal
['disk', 'Bitrix\Disk\BizProcDocument', '777'] – Disk file
['lists', 'BizprocDocument', '777'] – Process document in the feed (in news)
['lists', 'Bitrix\Lists\BizprocDocumentLists', '777'] – Lists document
```

Example of DOCUMENT_ID for a SPA:

```php
DOCUMENT_ID = ['crm', 'Bitrix\Crm\Integration\BizProc\Document\Dynamic', 'DYNAMIC_147_1']
```

Where 147 is the ID of the SPA, and 1 is the ID of the element.

Example of substitution for the DOCUMENT_ID parameter for new invoices:

```php
Bitrix\Crm\Integration\BizProc\Document\SmartInvoice
SMART_INVOICE_<element ID> 
```

To pass a user binding parameter in `PARAMETERS`, use the user identifier in the form of user_ID. For example:

```php
PARAMETERS: {
    'resp_employee': user_14 // Employee ID
}
```

{% include [Footnote on examples](../../_includes/examples.md) %}

{% note info "" %}

- Workflows can only be started from the REST API on paid plans, not on demo licenses or NFR licenses.

{% endnote %}