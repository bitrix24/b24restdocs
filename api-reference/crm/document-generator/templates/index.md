# Document Templates: Overview of Methods

A document template is a `.docx` file with placeholders that are populated with data from the CRM during generation. Each template is associated with a number generator and one or more types of CRM entities.

The template is uploaded via the API once. After that, it is used for each document generation: the template ID and a specific CRM entity—deal, contact, invoice, or Smart Process entity—are passed to the document creation method.

For example, a commercial estimate template can be linked to deals in the Sales Funnel `2_category_0`—when generating, the deal's data and the number from the number generator will be included in the document.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation:
> - [Document templates in CRM](https://helpdesk.bitrix24.com/open/25461591/)
> - [Edit standard document templates in CRM](https://helpdesk.bitrix24.com/open/25810499/)
> - [How to Add a Template and Create a Document Based on It](../../../../tutorials/crm/how-to-add-crm-objects/how-to-generate-documents.md)
> - [Create and upload your document template to CRM](https://helpdesk.bitrix24.com/open/18304486/)
> - [Custom fields in document templates](https://helpdesk.bitrix24.com/open/23479452/)
> - [Access permissions for CRM documents](https://helpdesk.bitrix24.com/open/25841971/)

## Getting Started

1. Prepare the template file in `.docx` format—[How to Upload Files](../../../files/how-to-upload-files.md).
2. Create a number generator or take the ID of an existing one—using methods from the [Number Generators](../numerator/index.md) section.
3. Upload the template using the [crm.documentgenerator.template.add](./crm-document-generator-template-add.md) method: pass the file, `numeratorId`, `region`, and `entityTypeId`.
4. Check the template fields using the [crm.documentgenerator.template.getfields](./crm-document-generator-template-get-fields.md) method if you need to verify the substitutions for a specific entity.
5. Create a document from the template using the [crm.documentgenerator.document.add](../documents/crm-document-generator-document-add.md) method, passing `templateId`, `entityTypeId`, and `entityId` of the specific CRM entity.

## Linking with Other Entities

**Number Generators.** The template is linked to a number generator through the `numeratorId` parameter. If you are creating a new number generator, take the `id` from the response of [crm.documentgenerator.numerator.add](../numerator/crm-document-generator-numerator-add.md). If you are using an existing one, obtain the `id` using the [crm.documentgenerator.numerator.list](../numerator/crm-document-generator-numerator-list.md) method.

**Documents.** The template serves as a basis for generating documents. When creating a document using the [crm.documentgenerator.document.add](../documents/crm-document-generator-document-add.md) method, the `templateId`, `entityTypeId`, and `entityId` of the specific CRM entity are passed to the method.

**CRM Entities.** The template is linked to one or more types of entities through `entityTypeId`. Typical values for CRM entities are provided in the article [Features of Passed Values](../index.md), and for Smart Processes, `entityTypeId` can be obtained using the [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md) method. To specify the funnel, a suffix is added to the identifier, for example, `2_category_0`.

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "modify" permission for document generator templates

#| 
|| **Method** | **Description** ||
|| [crm.documentgenerator.template.add](./crm-document-generator-template-add.md) | Adds a new template ||
|| [crm.documentgenerator.template.update](./crm-document-generator-template-update.md) | Updates an existing template ||
|| [crm.documentgenerator.template.get](./crm-document-generator-template-get.md) | Returns information about the template by ID ||
|| [crm.documentgenerator.template.list](./crm-document-generator-template-list.md) | Returns a list of templates ||
|| [crm.documentgenerator.template.delete](./crm-document-generator-template-delete.md) | Deletes a template ||
|| [crm.documentgenerator.template.getfields](./crm-document-generator-template-get-fields.md) | Returns the fields of the template for the specified CRM entity ||
|#