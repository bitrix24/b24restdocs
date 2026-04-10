# Document Generator: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The Document Generator compiles ready-made documents based on `.docx` templates and application data. It assists in uploading templates, retrieving their field maps, creating documents from them, and managing generation settings.

> Quick Navigation: [All Methods](#all-methods)

## How to Choose a Scenario

In Bitrix24, there are two groups of methods for working with the Document Generator: `documentgenerator.*` and `crm.documentgenerator.*`.

Templates, documents, and input data created through `documentgenerator.*` do not provide access to CRM data. For CRM scenarios, use the section [Document Generator in CRM](../crm/document-generator/index.md).

#|
||  | Methods `documentgenerator.*` | Methods `crm.documentgenerator.*` ||
|| Where to Use | In REST application scenarios, when templates and documents are within the `documentgenerator` scope | In CRM scenarios, when the document is linked to a CRM object ||
|| What to Pass During Generation | 
- `value` — external ID of the object for which the document is created
- `providerClassName` — data provider class | 
- `entityId` — CRM object identifier
- `entityTypeId` — CRM object type ||
|| Template Methods | [documentgenerator.template.*](./templates/index.md) | [crm.documentgenerator.template.*](../crm/document-generator/templates/index.md) ||
|| Where Results are Visible | At the REST level and in application logic | In scenarios and the CRM interface ||
|#

## How to Get Started

To work with documents in the REST scenario of the Document Generator:

1. Prepare a `.docx` template file with field placeholders.
2. Create or select a [numerator](./numerators/index.md) and [region](./region/index.md), if needed for the template.
3. Upload the template using the methods [documentgenerator.template.add](./templates/document-generator-template-add.md) or [documentgenerator.template.update](./templates/document-generator-template-update.md).
4. Retrieve the template's field map using the method [documentgenerator.template.getfields](./templates/document-generator-template-get-fields.md).
5. Create a document using the method [documentgenerator.document.add](./document-generator-document-add.md).
6. Retrieve the document using the method [documentgenerator.document.get](./document-generator-document-get.md) if you need to check its status and obtain file links.
7. Use [documentgenerator.document.list](./document-generator-document-list.md) if you need to find documents by template, external identifier, or other fields.
8. Enable a public link using the method [documentgenerator.document.enablepublicurl](./document-generator-document-enable-public-url.md) if the document needs to be accessed outside your Bitrix24.
9. Update the document using the method [documentgenerator.document.update](./document-generator-document-update.md) or delete it using the method [documentgenerator.document.delete](./document-generator-document-delete.md) if required by the scenario.
10. Configure access permissions through the [Roles and Permissions](./role/index.md) section if the application needs to manage access to templates and documents.

{% note tip "Typical Use-Cases and Scenarios" %}

- [{#T}](./examples/index.md)
- [{#T}](./examples/document-images-seals.md)
- [{#T}](./examples/document-date-name.md)
- [{#T}](./examples/document-table-data.md)
- [{#T}](./examples/document-table-complex.md)

{% endnote %}

## Linking to Other Objects

**Document Templates.** A template defines the `.docx` file, region, numerator, and a set of settings that are then used when creating a document. To work with templates, use the group of methods [documentgenerator.template.*](./templates/index.md).

**Numerators.** A numerator defines the rule for generating the document number, which the template uses through the `numeratorId` field. You can work with numerators using the group of methods [documentgenerator.numerator.*](./numerators/index.md).

**Regions.** A region defines the local settings of the template through the `region` field. For a pre-installed region, use `code`, and for a custom one — `id`. Management of regions is performed using the group of methods [documentgenerator.region.*](./region/index.md).

**Roles and Permissions.** Access permissions determine who can modify templates, documents, and Document Generator settings. To configure roles, use the group of methods [documentgenerator.role.*](./role/index.md).

## Feature of Document Conversion to PDF

The conversion of a file to PDF is performed asynchronously. If the `pdfUrl` field is not filled immediately after the document is created, call the method [documentgenerator.document.get](./document-generator-document-get.md) to check the conversion result again.

## Overview of Methods {#all-methods}

> Scope: [`documentgenerator`](../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

### Documents

#|
|| **Method** | **Description** ||
|| [documentgenerator.document.add](./document-generator-document-add.md) | Creates a new document based on a template ||
|| [documentgenerator.document.update](./document-generator-document-update.md) | Modifies an existing document ||
|| [documentgenerator.document.get](./document-generator-document-get.md) | Retrieves a document by its identifier ||
|| [documentgenerator.document.list](./document-generator-document-list.md) | Retrieves a list of documents ||
|| [documentgenerator.document.delete](./document-generator-document-delete.md) | Deletes a document ||
|| [documentgenerator.document.enablepublicurl](./document-generator-document-enable-public-url.md) | Enables or disables a public link to the document ||
|| [documentgenerator.document.getfields](./document-generator-document-get-fields.md) | Retrieves a list of document fields ||
|#

### Templates

#|
|| **Method** | **Description** ||
|| [documentgenerator.template.add](./templates/document-generator-template-add.md) | Uploads a new document template ||
|| [documentgenerator.template.update](./templates/document-generator-template-update.md) | Updates an existing document template ||
|| [documentgenerator.template.get](./templates/document-generator-template-get.md) | Returns a document template by its identifier ||
|| [documentgenerator.template.list](./templates/document-generator-template-list.md) | Returns a list of document templates by filter ||
|| [documentgenerator.template.delete](./templates/document-generator-template-delete.md) | Deletes a document template ||
|| [documentgenerator.template.getfields](./templates/document-generator-template-get-fields.md) | Returns the field map of the template ||
|#

### Numerators

#|
|| **Method** | **Description** ||
|| [documentgenerator.numerator.add](./numerators/document-generator-numerator-add.md) | Adds a numerator ||
|| [documentgenerator.numerator.update](./numerators/document-generator-numerator-update.md) | Modifies a numerator ||
|| [documentgenerator.numerator.get](./numerators/document-generator-numerator-get.md) | Retrieves a numerator by its identifier ||
|| [documentgenerator.numerator.list](./numerators/document-generator-numerator-list.md) | Retrieves a list of numerators ||
|| [documentgenerator.numerator.delete](./numerators/document-generator-numerator-delete.md) | Deletes a numerator ||
|#

### Regions

#|
|| **Method** | **Description** ||
|| [documentgenerator.region.add](./region/document-generator-region-add.md) | Adds a custom region ||
|| [documentgenerator.region.update](./region/document-generator-region-update.md) | Updates a custom region ||
|| [documentgenerator.region.get](./region/document-generator-region-get.md) | Returns region data by identifier or code ||
|| [documentgenerator.region.list](./region/document-generator-region-list.md) | Returns a list of pre-installed and custom regions ||
|| [documentgenerator.region.delete](./region/document-generator-region-delete.md) | Deletes a custom region ||
|#

### Roles and Permissions

#|
|| **Method** | **Description** ||
|| [documentgenerator.role.add](./role/document-generator-role-add.md) | Adds a role and returns its data with permissions ||
|| [documentgenerator.role.update](./role/document-generator-role-update.md) | Updates a role and returns the current role data ||
|| [documentgenerator.role.get](./role/document-generator-role-get.md) | Returns a role by its identifier along with permissions ||
|| [documentgenerator.role.list](./role/document-generator-role-list.md) | Returns a list of roles without detailed permission composition ||
|| [documentgenerator.role.delete](./role/document-generator-role-delete.md) | Deletes a role by its identifier ||
|| [documentgenerator.role.fillaccesses](./role/document-generator-role-fill-accesses.md) | Completely overwrites role bindings to access codes ||
|#