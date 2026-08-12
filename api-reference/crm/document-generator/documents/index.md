# Documents: Overview of Methods

A document is a file that the [document generator](../index.md) has built from a `.docx` template and the data of a CRM object. The finished document is retained in Bitrix24 in three formats: the source DOCX, a PDF, and a preview image.

A document is always linked to a specific CRM object — a deal, lead, contact, company, invoice, estimate, or SPA element. The link is defined by the `entityTypeId` and `entityId` pair.

For example, you can generate a document for deal No. 1042 from an invoice template and enable a public link for it to send to the client.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Documents in CRM: Create and send to customers](https://helpdesk.bitrix24.com/open/19441484/)

## How to Create a Document

1. Obtain the `templateId` — the identifier of a template from the [Document Templates](../templates/index.md) section.
2. Determine the `entityTypeId` and `entityId` of the CRM object for which the document is needed.
3. Create the document using the [crm.documentgenerator.document.add](./crm-document-generator-document-add.md) method. If the file is already prepared outside Bitrix24, upload it using the [crm.documentgenerator.document.upload](./crm-document-generator-document-upload.md) method.
4. Take the document `id` and the links to the files from the response.

To insert your own values into the document, pass them in the `values` parameter of the `crm.documentgenerator.document.add` method. The list of keys allowed for a specific document is returned by [crm.documentgenerator.document.getfields](./crm-document-generator-document-get-fields.md).

{% note tip "Typical use-cases and scenarios" %}

- [How to add a template and create a document based on it](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)

{% endnote %}

## How to Share a Document

By default, the document is available only to employees who have permissions for the CRM object. The [crm.documentgenerator.document.enablepublicurl](./crm-document-generator-document-enable-public-url.md) method enables a public link — the document opens through it without authorization in Bitrix24. The same method disables the link again.

## Important Considerations

- The `pdfUrl` and `imageUrl` links may not be available immediately after creating or updating the document, as the conversion is performed asynchronously. If you need the links right away, repeat the request using the [crm.documentgenerator.document.get](./crm-document-generator-document-get.md) method after 30-40 seconds.
- Permissions are checked per action: reading and listing require view permission for documents, [crm.documentgenerator.document.add](./crm-document-generator-document-add.md) requires create permission, and modifying and deleting require modify permission. Access to the document itself is checked additionally.
- The `crm.documentgenerator.document.*` methods work with CRM documents. If the document is not linked to a CRM object, use the methods of the [Document Generator](../../../document-generator/index.md) section.

## Relationships with Other Objects

**Document Templates.** To create a document from a template, pass the `templateId` to the [crm.documentgenerator.document.add](./crm-document-generator-document-add.md) method. Obtain the `templateId` from the response of [crm.documentgenerator.template.add](../templates/crm-document-generator-template-add.md) or retrieve it from the list using the [crm.documentgenerator.template.list](../templates/crm-document-generator-template-list.md) method.

**CRM Entities.** To create or upload a document, pass the `entityTypeId` and `entityId`. The values of `entityTypeId` for standard objects are provided in the table [CRM Object Types](../../data-types.md#object_type). For SPAs, the `entityTypeId` can be obtained using the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. The identifier of the required object, `entityId`, can be obtained using the universal [crm.item.list](../../universal/crm-item-list.md) method.

**Numbering.** When creating a document, the number is usually generated based on the numbering system associated with the template. If you are creating a new numbering system, obtain the `id` from the response of [crm.documentgenerator.numerator.add](../numerator/crm-document-generator-numerator-add.md). If you are using an existing one, retrieve the `id` using the [crm.documentgenerator.numerator.list](../numerator/crm-document-generator-numerator-list.md) method.

**Files.** In the [crm.documentgenerator.document.upload](./crm-document-generator-document-upload.md) method, the file content is passed in Base64. The upload format is described in the article [How to Upload Files](../../../files/how-to-upload-files.md).

## Overview of Methods and Events {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [crm.documentgenerator.document.add](./crm-document-generator-document-add.md) | Creates a document from a template ||
    || [crm.documentgenerator.document.update](./crm-document-generator-document-update.md) | Updates a document ||
    || [crm.documentgenerator.document.get](./crm-document-generator-document-get.md) | Returns information about the document ||
    || [crm.documentgenerator.document.list](./crm-document-generator-document-list.md) | Returns a list of documents ||
    || [crm.documentgenerator.document.delete](./crm-document-generator-document-delete.md) | Deletes a document ||
    || [crm.documentgenerator.document.enablepublicurl](./crm-document-generator-document-enable-public-url.md) | Enables or disables a public link ||
    || [crm.documentgenerator.document.upload](./crm-document-generator-document-upload.md) | Uploads a ready document and attaches it to a CRM object ||
    || [crm.documentgenerator.document.getfields](./crm-document-generator-document-get-fields.md) | Returns the fields of the created document ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [onCrmDocumentGeneratorDocumentAdd](./events/on-crm-document-generator-document-add.md) | When generating a document manually or using the [crm.documentgenerator.document.add](./crm-document-generator-document-add.md) and [crm.documentgenerator.document.upload](./crm-document-generator-document-upload.md) methods ||
    || [onCrmDocumentGeneratorDocumentUpdate](./events/on-crm-document-generator-document-update.md) | When modifying a document manually or using the [crm.documentgenerator.document.update](./crm-document-generator-document-update.md) method ||
    || [onCrmDocumentGeneratorDocumentDelete](./events/on-crm-document-generator-document-delete.md) | When deleting a document manually or using the [crm.documentgenerator.document.delete](./crm-document-generator-document-delete.md) method ||
    |#

{% endlist %}