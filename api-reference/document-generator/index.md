# About the Document Generator

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)

Currently, there are **two scopes** for working with the document generator:
- Methods `crm.documentgenerator.*`. The results of these methods are displayed in the CRM interface;
- Methods `documentgenerator.*`. The results of these methods are only available at the REST level.

You cannot access data from one scope using methods from another:
- You cannot create a CRM document with a template for REST;
- You cannot use CRM data when working with methods `documentgenerator.*`.

The following **field types** and their **modifiers** are supported:

- IMAGE - images;
- STAMP - seals and signatures;
- DATE - dates;
- NAME - names.

The field types **Money** and **Address** are implemented within the *crm* module, so they cannot be used in the REST of this module. If you need to output such data, you will have to pass it in a pre-formed format.

It is possible to use arrays for inserting into tables and repeating blocks.

## Differences in Method Parameters Across Scopes

The methods are identical "from the inside." In fact, the methods `crm.documentgenerator.*` call the methods `documentgenerator.*` after preprocessing the parameters. However, there are several differences:
- For the input of methods `crm.documentgenerator.*`, you need to pass the **ID** of the CRM entity type instead of provider names (parameter `entityTypeId`);
- For the input of methods `crm.documentgenerator.*`, you need to pass the parameter `entityId` - **ID** of the CRM entity instead of the **value** parameter.

## Templates

All templates and documents created by this API are tied to the REST module. You cannot access templates and documents from other modules through the `documentgenerator` scope. Therefore, the `moduleId` in the template data will always be `rest`. Even if you specify another module in `add` or `update`, it will not be changed.

Only two providers are available for REST:

- `Bitrix\DocumentGenerator\DataProvider\Rest` - must always be specified as the provider for the template;
- `Bitrix\DocumentGenerator\DataProvider\HashDataProvider` - used for passing data into tables / repeating blocks.

The binding of the template to the user is not considered by the REST methods themselves. However, it can be used on the application side.

## Numbering

To work with numbering, there are methods `documentgenerator.numerator.*`, described [here](./numerators/index.md). It should be noted that through this scope, you can access all numberers for documents, including those that work in CRM. However, you can only update/delete the numberer that was created through REST.

## List of Regions

Each template is tied to a specific country. The list of countries is fixed and currently consists of:

- us - United States
- by - Belarus
- kz - Kazakhstan
- ua - Ukraine
- br - Brazil
- mx - Mexico
- de - Germany
- uk - United Kingdom
- pl - Poland

Starting from version documentgenerator 18.6.1, it became possible to add your own regions. A [separate section](./region/index.md) has been created for managing them.

## Summary

**What can be done?**

- Create documents based on templates in .docx file format;
- Insert lists with an arbitrary number of items into the template through tables or repeating blocks;
- Insert images into the template, including from lists;
- Insert fields in HTML format with partial preservation of formatting;
- Create documents, send them, and track views without user involvement (through Automation rules).

**What cannot be done?**

- Insert multiple values for the "file" field type;
- Insert tables and images from HTML;
- Insert vector images;
- Formatting transfer is not fully executed.

## Overview of Methods and Events {#all-methods}

### Documents

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [documentgenerator.document.add](./document-generator-document-add.md) | Creates a new document based on a template ||
    || [documentgenerator.document.delete](./document-generator-document-delete.md) | Deletes a document ||
    || [documentgenerator.document.enablepublicurl](./document-generator-document-enable-public-url.md) | Enables/disables public link to the document ||
    || [documentgenerator.document.getfields](./document-generator-document-get-fields.md) | Retrieves a list of document fields ||
    || [documentgenerator.document.get](./document-generator-document-get.md) | Retrieves a document by its identifier ||
    || [documentgenerator.document.list](./document-generator-document-list.md) | Retrieves a list of documents ||
    || [documentgenerator.document.update](./document-generator-document-update.md) | Modifies an existing document ||
    |#

- Events

    #| 
    || **Event** | **Description** ||
    || [onCrmDocumentGeneratorDocumentAdd](./events/on-crm-document-generator-add.md) | On document generation ||
    || [onCrmDocumentGeneratorDocumentDelete](./events/on-crm-document-generator-document-delete.md) | On document deletion ||
    || [onCrmDocumentGeneratorDocumentUpdate](./events/on-crm-document-generator-document-update.md) | On document modification ||
    |#

{% endlist %}

### Numbering

#| 
|| **Method** | **Description** ||
|| [documentgenerator.numerator.add](./numerators/document-generator-numerator-add.md) | Adds a numberer ||
|| [documentgenerator.numerator.delete](./numerators/document-generator-numerator-delete.md) | Deletes a numberer ||
|| [documentgenerator.numerator.get](./numerators/document-generator-numerator-get.md) | Retrieves a numberer by its identifier ||
|| [documentgenerator.numerator.list](./numerators/document-generator-numerator-list.md) | Retrieves a list of numberers ||
|| [documentgenerator.numerator.update](./numerators/document-generator-numerator-update.md) | Modifies a numberer ||
|#

### Regions

#| 
|| **Method** | **Description** ||
|| [documentgenerator.region.get](./region/document-generator-region-get.md) | Returns information about a region by its identifier ||
|| [documentgenerator.region.list](./region/document-generator-region-list.md) | Returns a list of regions, both default and custom ||
|| [documentgenerator.region.delete](./region/document-generator-region-delete.md) | Deletes a region ||
|| [documentgenerator.region.add](./region/document-generator-region-add.md) | Adds a new region ||
|| [documentgenerator.region.update](./region/document-generator-region-update.md) | Updates an existing country ||
|#

### Roles

#| 
|| **Method** | **Description** ||
|| [documentgenerator.role.get](./role/document-generator-role-get.md) | Returns information about a role and its access permissions ||
|| [documentgenerator.role.list](./role/document-generator-role-list.md) | Returns a list of roles without their access permissions ||
|| [documentgenerator.role.delete](./role/document-generator-role-delete.md) | Deletes a role ||
|| [documentgenerator.role.add](./role/document-generator-role-add.md) | Adds a new role ||
|| [documentgenerator.role.update](./role/document-generator-role-update.md) | Updates roles ||
|| [documentgenerator.role.fillaccesses](./role/document-generator-role-fill-accesses.md) | Sets the set of roles and their bindings ||
|#

### Templates

#| 
|| **Method** | **Description** ||
|| [documentgenerator.template.add](./templates/document-generator-template-add.md) | Uploads a new document template ||
|| [documentgenerator.template.update](./templates/document-generator-template-update.md) | Updates an existing document template ||
|| [documentgenerator.template.get](./templates/document-generator-template-get.md) | Returns a document template by its identifier ||
|| [documentgenerator.template.list](./templates/document-generator-template-list.md) | Returns a list of document templates by filter ||
|| [documentgenerator.template.delete](./templates/document-generator-template-delete.md) | Deletes a document template ||
|| [documentgenerator.template.getfields](./templates/document-generator-template-get-fields.md) | Returns a list of document template fields ||
|#