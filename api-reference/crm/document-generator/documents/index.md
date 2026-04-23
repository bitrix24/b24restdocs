# Documents: Overview of Methods

Documents in the Bitrix24 document generator can be created from a template for a CRM entity or uploaded as a ready-made file and attached to a CRM entity.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Documents in CRM: Create and send to customers](https://helpdesk.bitrix24.com/open/19441484/)

## Getting Started

1. Prepare the document template and obtain the `templateId` in the [Document Templates](../templates/index.md) section.
2. Determine the `entityTypeId` of the CRM entity for which the document is needed.
3. Obtain the `entityId` of the required CRM entity.
4. If you need to check the available document fields, use the [crm.documentgenerator.document.getfields](./crm-document-generator-document-get-fields.md) method.
5. Create a document from the template using the [crm.documentgenerator.document.add](./crm-document-generator-document-add.md) method or upload a ready-made file using the [crm.documentgenerator.document.upload](./crm-document-generator-document-upload.md) method.
6. If you know the document's `id` and want to retrieve its data, use the [crm.documentgenerator.document.get](./crm-document-generator-document-get.md) method.
7. If you need to get a list of documents, use the [crm.documentgenerator.document.list](./crm-document-generator-document-list.md) method.
8. To modify a document, use the [crm.documentgenerator.document.update](./crm-document-generator-document-update.md) method.
9. If you need a public link to the document, use the [crm.documentgenerator.document.enablepublicurl](./crm-document-generator-document-enable-public-url.md) method.
10. Delete an unnecessary document using the [crm.documentgenerator.document.delete](./crm-document-generator-document-delete.md) method.

{% note tip "Typical use-cases and scenarios" %}

- [How to add a template and create a document based on it](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)

{% endnote %}

## Important Considerations

The `pdfUrl` and `imageUrl` links may not be available immediately after creating or updating the document, as the conversion is performed asynchronously. If you need the links right away, repeat the request using the [crm.documentgenerator.document.get](./crm-document-generator-document-get.md) method after 30-40 seconds.

## Connection with Other Objects

**Document Templates.** To create a document from a template, pass the `templateId` to the [crm.documentgenerator.document.add](./crm-document-generator-document-add.md) method. Obtain the `templateId` from the response of [crm.documentgenerator.template.add](../templates/crm-document-generator-template-add.md) or retrieve it from the list using the [crm.documentgenerator.template.list](../templates/crm-document-generator-template-list.md) method.

**CRM Entities.** To create or upload a document, pass the `entityTypeId` and `entityId`. Typical values for `entityTypeId` for CRM entities are provided in the article [Features of Passed Values](../index.md). For SPAs, the `entityTypeId` can be obtained using the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. The identifier of the required object, `entityId`, can be obtained using the universal [crm.item.list](../../universal/crm-item-list.md) method.

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
    || [crm.documentgenerator.document.upload](./crm-document-generator-document-upload.md) | Uploads a ready document and attaches it to a CRM entity ||
    || [crm.documentgenerator.document.getfields](./crm-document-generator-document-get-fields.md) | Returns the fields of the created document ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [onCrmDocumentGeneratorDocumentAdd](./events/on-crm-document-generator-document-add.md) | When generating a document manually or using the [crm.documentgenerator.document.add](./crm-document-generator-document-add.md) method ||
    || [onCrmDocumentGeneratorDocumentUpdate](./events/on-crm-document-generator-document-update.md) | When modifying a document manually or using the [crm.documentgenerator.document.update](./crm-document-generator-document-update.md) method ||
    || [onCrmDocumentGeneratorDocumentDelete](./events/on-crm-document-generator-document-delete.md) | When deleting a document manually or using the [crm.documentgenerator.document.delete](./crm-document-generator-document-delete.md) method ||
    |#

{% endlist %}