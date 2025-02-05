# Document Generator

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- An introduction corresponding to the title is needed.

{% endnote %}

{% endif %}

{% note info "Permissions" %}

**Scope**: [`crm.documentgenerator`](../../scopes/permissions.md) | **Who can execute the method**: `any user`

> Quick navigation: [all methods](#all-methods) 

{% endnote %}

The CRM REST methods for documents are implemented through AJAX controllers. The controllers and their actions are implemented in the [Document Generator](../document-generator/index.md) module, while the CRM module only provides the wrapper for them. This wrapper transforms the input and output parameters according to the new REST standard, taking into account the specifics of the CRM module.

When working with document methods through the CRM scope, you can only work with templates/documents linked to the CRM module.

## Features of Passed Values

### Providers

The **Document Generator** module uses the full class name as the data provider identifier. Since the CRM module uses named/numeric identifiers to identify entities, a similar syntax was used in REST for documents. If the input parameter is called *entityTypeId*, it accepts the numeric index of CRM entities. Currently, the following identifiers are available:

- 1 - lead
- 2 - deal
- 3 - contact
- 4 - company
- 5 - invoice
- 7 - estimate
- 14 - order
- 31 - new invoices

It is also possible to use SPAs: their *entityTypeId* is also supported in the Document Generator.

### Filtering by Deal Directions

REST methods do not consider the template binding to providers. That is, if a request is sent to generate a document based on a template with a provider to which this template is not linked, there will be no error. It follows that the binding of the template to a specific deal direction is not taken into account. If you want to specify a deal as a provider, only the numeric identifier (2) is indicated.

### List of Regions

Each template is linked to a specific country. The list of countries is fixed and currently consists of:

- de - Germany
- by - Belarus
- kz - Kazakhstan
- ua - Ukraine
- br - Brazil
- mx - Mexico
- uk - United Kingdom
- pl - Poland

Region management is carried out in the [documentgenerator](../../document-generator/region/index.md) section.

## Overview of Methods {#all-methods}

### Documents

#| 
|| **Method** | **Description** ||
|| [crm.documentgenerator.document.getfields](./documents/crm-document-generator-document-get-fields.md) | Document fields. ||
|| [crm.documentgenerator.document.add](./documents/crm-document-generator-document-add.md) | Create a new document. ||
|| [crm.documentgenerator.document.update](./documents/crm-document-generator-document-update.md) | Modify a document. ||
|| [crm.documentgenerator.document.get](./documents/crm-document-generator-document-get.md) | Retrieve information about a document by Id. ||
|| [crm.documentgenerator.document.list](./documents/crm-document-generator-document-list.md) | Get a list of documents. ||
|| [crm.documentgenerator.document.enablepublicurl](./documents/crm-document-generator-document-enable-public-url.md) | Enable and disable public link to the document. ||
|| [crm.documentgenerator.document.upload](./documents/crm-document-generator-document-upload.md) | Upload the generated document and attach it to the specified entity. ||
|| [crm.documentgenerator.document.delete](./documents/crm-document-generator-document-delete.md) | Delete a document. ||
|#

### Numerator

#| 
|| **Method** | **Description** ||
|| [crm.documentgenerator.numerator.add](./numerator/crm-document-generator-numerator-add.md) | Add a new numerator. ||
|| [crm.documentgenerator.numerator.update](./numerator/crm-document-generator-numerator-update.md) | Modify an existing numerator. ||
|| [crm.documentgenerator.numerator.get](./numerator/crm-document-generator-numerator-get.md) | Retrieve information about a numerator by Id. ||
|| [crm.documentgenerator.numerator.list](./numerator/crm-document-generator-numerator-list.md) | Get a list of numerators. ||
|| [crm.documentgenerator.numerator.delete](./numerator/crm-document-generator-numerator-delete.md) | Delete a numerator. ||
|#

### Document Templates

#| 
|| **Method** | **Description** ||
|| [crm.documentgenerator.template.getfields](./templates/crm-document-generator-template-get-fields.md) | Document template fields. ||
|| [crm.documentgenerator.template.add](./templates/crm-document-generator-template-add.md) | Add a new template. ||
|| [crm.documentgenerator.template.update](./templates/crm-document-generator-template-update.md) | Modify an existing document template. ||
|| [crm.documentgenerator.template.get](./templates/crm-document-generator-template-get.md) | Retrieve information about a document template by Id. ||
|| [crm.documentgenerator.template.list](./templates/crm-document-generator-template-list.md) | Get a list of document templates. ||
|| [crm.documentgenerator.template.delete](./templates/crm-document-generator-template-delete.md) | Delete a document template. ||
|#