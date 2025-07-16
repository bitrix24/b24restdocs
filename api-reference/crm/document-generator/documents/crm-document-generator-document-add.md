# Create a new document crm.documentgenerator.document.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- it's better to add a custom response in case of success (not as a link)
- no response in case of error

{% endnote %}

{% endif %}

> Scope: [`crm.documentgenerator`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.documentgenerator.document.add` creates a new document based on a template and data from the corresponding entity. In `values`, you can pass an array of additional field values.

#|
|| **Parameter** | **Description** ||
|| **templateId** | Template ID. ||
|| **entityTypeId** | CRM entity type ID. ||
|| **entityId** | Entity ID. ||
|| **values** | Additional field values. ||
|| **stampsEnabled** | 1 (enable), 0 (disable) stamps and signatures. ||
|#

## Response in case of success

In case of successful execution, the response will be a structure similar to the method [crm.documentgenerator.document.get()](./crm-document-generator-document-get.md) for the new document.

### Why there is no link to **pdf** in the result of **crm.documentgenerator.document.add**

- Conversion to **pdf** is an asynchronous operation. At the time the document generation is completed, the **pdf** file is not yet available. The minimum conversion time is 8 seconds.
- If a **pdf** is urgently needed for the document, the only option right now is to make a repeated request to **crm.documentgenerator.document.get** after 20-30 seconds to retrieve the link to the **pdf**. If it hasn't appeared, try again.

## See also

- [Document generation examples](../../../document-generator/examples/index.md)
- [Presentation on the document generator](https://dev.quickbooks.com/upload/doc_gen_04_10_2019.pdf)
- [Document templates](https://helpdesk.bitrix24.com/open/19441484/)

## Continue your study

- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)