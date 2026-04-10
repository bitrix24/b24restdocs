# Document Generator Templates: Overview of Methods

A template serves as the foundation for creating a document: a `.docx` file with placeholders, a region, a numerator, and settings.

> Quick Navigation: [All Methods](#all-methods)

## Requirements for the Template File

The template file must be in `.docx` format and contain placeholders for fields that the document generator will replace with data when creating the document.

Values for substitution are formatted within curly braces, such as `{DocumentNumber}` or `{MyField}`.

Supported field types include `IMAGE`, `STAMP`, `DATE`, and `NAME`. The Money and Address fields pertain to the CRM module, so in the REST template, such values need to be passed in a prepared format.

## Getting Started

1. Prepare a `.docx` template file.
2. Obtain or create a numerator in the [Numerators](../numerators/index.md) section.
3. Select a pre-defined region or create a custom one in the [Regions](../region/index.md) section.
4. Create a template using the [documentgenerator.template.add](./document-generator-template-add.md) method.
5. Modify the template using the [documentgenerator.template.update](./document-generator-template-update.md) method if you need to update the file or settings.
6. Check the template data using the [documentgenerator.template.get](./document-generator-template-get.md) method or find it in the list via [documentgenerator.template.list](./document-generator-template-list.md).
7. Retrieve the template field card using the [documentgenerator.template.getfields](./document-generator-template-get-fields.md) method.
8. Use the template ID when [creating a new document](../document-generator-document-add.md).

## Linking Templates with Other Objects

**Documents.** The template is used as the basis for a document. The template identifier `templateId` is passed to the document management methods `documentgenerator.document.*`. You can create a document from a template using the [documentgenerator.document.add](../document-generator-document-add.md) method.

**Numerators.** The template is linked to a numerator through the `numeratorId` field. You can access available numerators using the [documentgenerator.numerator.list](../numerators/document-generator-numerator-list.md) method, and create a new one using the [documentgenerator.numerator.add](../numerators/document-generator-numerator-add.md) method.

**Regions.** Local settings for the template are defined by the `region` field. The required value for this field can be obtained using the [documentgenerator.region.list](../region/document-generator-region-list.md) method.

**Roles and Permissions.** The methods `documentgenerator.template.*` are available to users with permission to modify document generator templates. Permissions for working with templates are configured through the [Roles](../role/index.md) section.

## Considerations When Working with Templates

All templates in this section are created in the `documentgenerator` scope and pertain to the `rest` module. The `moduleId` field for them always remains `rest`, so these methods cannot be used to manage templates from other modules.

When creating a template, providers for the REST scenario are filled in automatically. The base provider is `Bitrix\DocumentGenerator\DataProvider\Rest`. To pass arrays into tables and repeating blocks, `Bitrix\DocumentGenerator\DataProvider\HashDataProvider` is used.

The fields `users`, `active`, and `sort` relate to the application's own settings. They help manage the visibility and order of templates in the application's interface but do not create a ready-made user interface on the Bitrix24 side.

## Features of Remote Templates

If documents have already been created from a template, the [documentgenerator.template.delete](./document-generator-template-delete.md) method does not completely remove it but marks it as deleted. Such templates can be retrieved through [documentgenerator.template.list](./document-generator-template-list.md) with the filter `isDeleted = "Y"`. This allows for the preservation of document bindings.

For deleted templates, you cannot retrieve the field card using the [documentgenerator.template.getfields](./document-generator-template-get-fields.md) method, so it is advisable to check the `isDeleted` value before further processing.

## Overview of Methods {#all-methods}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the methods: users with permission to modify document generator templates

#|
|| **Method** | **Description** ||
|| [documentgenerator.template.add](./document-generator-template-add.md) | Uploads a new document template ||
|| [documentgenerator.template.update](./document-generator-template-update.md) | Updates an existing document template ||
|| [documentgenerator.template.get](./document-generator-template-get.md) | Returns a document template by identifier ||
|| [documentgenerator.template.list](./document-generator-template-list.md) | Returns a list of document templates by filter ||
|| [documentgenerator.template.delete](./document-generator-template-delete.md) | Deletes a document template ||
|| [documentgenerator.template.getfields](./document-generator-template-get-fields.md) | Returns the field card of the template ||
|#