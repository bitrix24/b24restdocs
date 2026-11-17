# Documents: Overview of Methods

A CRM document can be created from a `.docx` template and CRM object data or uploaded as a ready-made file. A PDF and a preview image may also be available for the document.

A document is linked to a specific CRM object, such as a deal, lead, contact, company, invoice, estimate, or SPA item. The link is defined by the `entityTypeId` and `entityId` pair.

For example, you can generate a document for deal No. 1042 from an invoice template and enable a public link for it to send to the client.

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Documents in CRM: Create and send to customers](https://helpdesk.bitrix24.com/open/19441484/)

## How to Create a Document

To create a document from a template, retrieve the `templateId` from the [Document Templates](../templates/index.md) section, determine the `entityTypeId` and `entityId` of the CRM object, and call [crm.documentgenerator.document.add](./crm-document-generator-document-add.md).

If the file is already prepared, use [crm.documentgenerator.document.upload](./crm-document-generator-document-upload.md) and pass the upload parameters in `fields`. The identifier of the created document is returned in `result.document.id`.

To insert your own values into the document, pass them in the `values` parameter of the `crm.documentgenerator.document.add` method. The list of keys allowed for a specific document is returned by [crm.documentgenerator.document.getfields](./crm-document-generator-document-get-fields.md).

{% note tip "Typical use-cases and scenarios" %}

- [How to add a template and create a document based on it](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)

{% endnote %}

## How to Share a Document

By default, the document is available only to employees who have permissions for the CRM object. The [crm.documentgenerator.document.enablepublicurl](./crm-document-generator-document-enable-public-url.md) method enables a public link — the document opens through it without authorization in Bitrix24. The same method disables the link again.

## Important Considerations

- The `pdfUrl` and `imageUrl` links may not be available immediately after creating or updating the document, as the conversion is performed asynchronously. If you need the links right away, repeat the request using the [crm.documentgenerator.document.get](./crm-document-generator-document-get.md) method after 30-40 seconds.
- Document methods require permission to view documents. The `add` method also checks permission to create a document from a template; `update`, `delete`, `getfields`, and `enablepublicurl` check permission to modify the specific document. Methods that accept a document also check access to it.
- When uploading a ready-made file, the `upload` method returns an error starting with `Empty required fields:` if required fields are missing, or `Wrong "entityTypeId" field value` if the CRM object type is invalid. For details, see the [Method Description](./crm-document-generator-document-upload.md).
- The `crm.documentgenerator.document.*` methods work with CRM documents. If the document is not linked to a CRM object, use the methods of the [Document Generator](../../../document-generator/index.md) section.

## Relationships with Other Objects

**Document Templates.** Retrieve the template identifier using [crm.documentgenerator.template.list](../templates/crm-document-generator-template-list.md).

**CRM Entities.** To create or upload a document, pass the `entityTypeId` and `entityId`. The values of `entityTypeId` for standard objects are provided in the table [CRM Object Types](../../data-types.md#object_type). For SPAs, the `entityTypeId` can be obtained using the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. The identifier of the required object, `entityId`, can be retrieved using [crm.item.list](../../universal/crm-item-list.md) for the object types it supports; for legacy invoices, use [crm.invoice.list](../../outdated/invoice/crm-invoice-list.md).

**Numbering.** The document number is generated using the numbering rule linked to the template. For configuration details, see [Numbering Rules](../numerator/index.md).

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
    || [onCrmDocumentGeneratorDocumentAdd](./events/on-crm-document-generator-document-add.md) | When a document is created manually or using [crm.documentgenerator.document.add](./crm-document-generator-document-add.md), or a ready-made document is uploaded using [crm.documentgenerator.document.upload](./crm-document-generator-document-upload.md) ||
    || [onCrmDocumentGeneratorDocumentUpdate](./events/on-crm-document-generator-document-update.md) | When modifying a document manually or using the [crm.documentgenerator.document.update](./crm-document-generator-document-update.md) method ||
    || [onCrmDocumentGeneratorDocumentDelete](./events/on-crm-document-generator-document-delete.md) | When deletion of a document is attempted manually or using [crm.documentgenerator.document.delete](./crm-document-generator-document-delete.md). The event is generated before deletion is complete ||
    |#

{% endlist %}

Subscription details and notification data are described in the [Document Events Overview](./events/index.md).
