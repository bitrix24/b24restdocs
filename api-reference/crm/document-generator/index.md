# Document Generator: Overview of Methods and Events

The document generator in CRM allows you to prepare a template, configure a numbering system, create a document from a template, or upload a ready-made file. It supports deals, leads, contacts, companies, invoices, estimates, and elements of Smart Process Automation (SPA).

The section consists of three main parts. In [Numerators](./numerator/index.md), you configure the number template and counter. In [Document Templates](./templates/index.md), you upload a `.docx` file, link it to the numerator, region, and types of CRM entities. In [Documents](./documents/index.md), you create a document from a template or upload a ready-made file, obtain links to the file, include a public link, and modify or delete the document.

For example, you can create a numerator for deals, link it to the deal template, and then create documents for specific deals.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../sdk/mcp.md) so that the assistant uses the official REST documentation.

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation:
> - [Documents in CRM: Create and send to customers](https://helpdesk.bitrix24.com/open/19441484/)
> - [Document templates in CRM](https://helpdesk.bitrix24.com/open/25461591/)
> - [Create and configure auto numbering template in CRM](https://helpdesk.bitrix24.com/open/25809783/)

## Getting Started

1. Create a numerator using the method [crm.documentgenerator.numerator.add](./numerator/crm-document-generator-numerator-add.md) — this sets the number template for documents.
2. Prepare a `.docx` template file in Base64 format — [How to upload files](../../files/how-to-upload-files.md).
3. Define the `entityTypeId` of the required CRM entity — typical values are provided in the article [Features of transmitted values](../index.md).
4. Upload the template using the method [crm.documentgenerator.template.add](./templates/crm-document-generator-template-add.md): pass the name, file, `numeratorId`, `entityTypeId`, and `region`.
5. Obtain the `entityId` of the required CRM entity using the method [crm.item.list](../universal/crm-item-list.md).
6. Choose how to work with the document:
   - to create a document from a template, use [crm.documentgenerator.document.add](./documents/crm-document-generator-document-add.md).
   - to upload a ready-made file, use [crm.documentgenerator.document.upload](./documents/crm-document-generator-document-upload.md).

{% note tip "Typical use-cases and scenarios" %}

- [How to add a template and create a document based on it](../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)

{% endnote %}

## Important Considerations

- When creating a document from a template, the link between the template and the Sales Funnel is not automatically checked — the document is created even if the template is configured for a different funnel.
- The `pdfUrl` and `imageUrl` links may be absent immediately after creating or updating the document, as conversion is performed asynchronously. If you need the links right away, repeat the request using the method [crm.documentgenerator.document.get](./documents/crm-document-generator-document-get.md) after 30-40 seconds.

## Relationships with Other Objects

**Numerators.** The numerator sets the number template and counter for documents. The document template uses it via the `numeratorId` parameter. If you are creating a new numerator, take the `id` from the response of [crm.documentgenerator.numerator.add](./numerator/crm-document-generator-numerator-add.md). If you are using an existing one, obtain the `id` using the method [crm.documentgenerator.numerator.list](./numerator/crm-document-generator-numerator-list.md).

**Document Templates.** The template stores the `.docx` file, region, linkage to types of CRM entities, and connection with the numerator. To create a document, you need the `templateId` — take the `id` from the response of [crm.documentgenerator.template.add](./templates/crm-document-generator-template-add.md) or the `id` of the required template from the response of [crm.documentgenerator.template.list](./templates/crm-document-generator-template-list.md).

**Documents.** A document is created from a template using the method [crm.documentgenerator.document.add](./documents/crm-document-generator-document-add.md) or a ready-made file is uploaded using the method [crm.documentgenerator.document.upload](./documents/crm-document-generator-document-upload.md) — in both cases, the document is attached to a CRM entity.

**CRM Entities.** Templates and documents use `entityTypeId` and `entityId`. Typical values of `entityTypeId` for CRM entities are provided in the article [Features of transmitted values](../index.md). For Smart Processes, `entityTypeId` can be obtained using the method [crm.type.list](../universal/user-defined-object-types/crm-type-list.md). The identifier of the required object `entityId` is obtained using the method [crm.item.list](../universal/crm-item-list.md).

**Regions.** The template is linked to the country via the `region` parameter. The `region` value is passed in the method [crm.documentgenerator.template.add](./templates/crm-document-generator-template-add.md), for example, `de`. A list of available regions can be obtained using the method [documentgenerator.region.list](../../document-generator/region/document-generator-region-list.md).

**Files.** Templates use a `.docx` file, and in the method [crm.documentgenerator.document.upload](./documents/crm-document-generator-document-upload.md), the content of the ready-made DOCX file is passed in Base64. The upload format is described in the article [How to upload files](../../files/how-to-upload-files.md).

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can perform the method: depends on the method

### Numerators

#| 
|| **Method** | **Description** ||
|| [crm.documentgenerator.numerator.add](./numerator/crm-document-generator-numerator-add.md) | Adds a new numerator ||
|| [crm.documentgenerator.numerator.update](./numerator/crm-document-generator-numerator-update.md) | Updates an existing numerator ||
|| [crm.documentgenerator.numerator.get](./numerator/crm-document-generator-numerator-get.md) | Returns information about the numerator by identifier ||
|| [crm.documentgenerator.numerator.list](./numerator/crm-document-generator-numerator-list.md) | Returns a list of numerators ||
|| [crm.documentgenerator.numerator.delete](./numerator/crm-document-generator-numerator-delete.md) | Deletes a numerator ||
|#

### Document Templates

#| 
|| **Method** | **Description** ||
|| [crm.documentgenerator.template.add](./templates/crm-document-generator-template-add.md) | Adds a new template ||
|| [crm.documentgenerator.template.update](./templates/crm-document-generator-template-update.md) | Updates an existing template ||
|| [crm.documentgenerator.template.get](./templates/crm-document-generator-template-get.md) | Returns information about the template by identifier ||
|| [crm.documentgenerator.template.list](./templates/crm-document-generator-template-list.md) | Returns a list of templates ||
|| [crm.documentgenerator.template.delete](./templates/crm-document-generator-template-delete.md) | Deletes a template ||
|| [crm.documentgenerator.template.getfields](./templates/crm-document-generator-template-get-fields.md) | Returns the fields of the template for the specified CRM entity ||
|#

### Documents

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [crm.documentgenerator.document.add](./documents/crm-document-generator-document-add.md) | Creates a document from a template ||
    || [crm.documentgenerator.document.update](./documents/crm-document-generator-document-update.md) | Updates a document ||
    || [crm.documentgenerator.document.get](./documents/crm-document-generator-document-get.md) | Returns information about the document ||
    || [crm.documentgenerator.document.list](./documents/crm-document-generator-document-list.md) | Returns a list of documents ||
    || [crm.documentgenerator.document.delete](./documents/crm-document-generator-document-delete.md) | Deletes a document ||
    || [crm.documentgenerator.document.enablepublicurl](./documents/crm-document-generator-document-enable-public-url.md) | Enables or disables the public link ||
    || [crm.documentgenerator.document.upload](./documents/crm-document-generator-document-upload.md) | Uploads a ready document and attaches it to a CRM entity ||
    || [crm.documentgenerator.document.getfields](./documents/crm-document-generator-document-get-fields.md) | Returns the fields of the created document ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [onCrmDocumentGeneratorDocumentAdd](./documents/events/on-crm-document-generator-document-add.md) | When generating a document manually or using the method [crm.documentgenerator.document.add](./documents/crm-document-generator-document-add.md) ||
    || [onCrmDocumentGeneratorDocumentUpdate](./documents/events/on-crm-document-generator-document-update.md) | When modifying a document manually or using the method [crm.documentgenerator.document.update](./documents/crm-document-generator-document-update.md) ||
    || [onCrmDocumentGeneratorDocumentDelete](./documents/events/on-crm-document-generator-document-delete.md) | When deleting a document manually or using the method [crm.documentgenerator.document.delete](./documents/crm-document-generator-document-delete.md) ||
    |#

{% endlist %}