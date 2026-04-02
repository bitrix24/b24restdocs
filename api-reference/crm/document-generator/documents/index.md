# Documents

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- An introduction corresponding to the title is needed.

{% endnote %}

{% endif %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for document generator documents.

#| 
|| **Method** | **Description** ||
|| [crm.documentgenerator.document.add](./crm-document-generator-document-add.md) | Creates a new document ||
|| [crm.documentgenerator.document.update](./crm-document-generator-document-update.md) | Updates a document ||
|| [crm.documentgenerator.document.get](./crm-document-generator-document-get.md) | Returns information about a document by ID ||
|| [crm.documentgenerator.document.list](./crm-document-generator-document-list.md) | Returns a list of documents ||
|| [crm.documentgenerator.document.delete](./crm-document-generator-document-delete.md) | Deletes a document ||
|| [crm.documentgenerator.document.enablepublicurl](./crm-document-generator-document-enable-public-url.md) | Enables or disables a public link to the document ||
|| [crm.documentgenerator.document.upload](./crm-document-generator-document-upload.md) | Uploads a completed document and attaches it to a CRM entity ||
|| [crm.documentgenerator.document.getfields](./crm-document-generator-document-get-fields.md) | Returns the fields of the document ||
|#