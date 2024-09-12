# About the Document Generator

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

The description of the [Document Generator](https://training.bitrix24.com/api_d7/bitrix/documentgenerator/index.php) module can be found in the D7 documentation.

Architecturally, the REST API largely corresponds to the PHP API of the module. The set of REST methods covers all the capabilities of the module.

Currently, there are **two scopes** for working with the document generator:
- Methods `crm.documentgenerator.*`. The results of these methods are displayed in the CRM interface;
- Methods `documentgenerator.*`. The results of these methods are only available at the REST level.

Methods from one scope cannot access data from another:
- You cannot create a CRM document with a template for REST;
- You cannot use CRM data when working with the documentgenerator.* methods.

The following **field types** and their **modifiers** are supported:

- IMAGE - images;
- STAMP - seals and signatures;
- DATE - dates;
- NAME - names.

The field types **Money** and **Address** are implemented within the *crm* module, so they cannot be used in the REST of this module. If you need to display such data, you will have to pass it in a pre-formed format.

It is possible to use arrays for insertion into tables and repeating blocks.

## Differences in Method Parameters Across Scopes

“Internally,” the methods are identical. In fact, the methods `crm.documentgenerator.*` call the methods `documentgenerator.*` after preprocessing the parameters. However, there are several differences:
- For the methods `crm.documentgenerator.*`, you need to pass the **ID** of the CRM entity type (parameter `entityTypeId`) instead of provider names;
- For the methods `crm.documentgenerator.*`, you need to pass the `entityId` parameter - the **ID** of the CRM entity instead of the **value** parameter.

## Templates

All templates and documents created by this API are tied to the REST module. You cannot access templates and documents from other modules through the `documentgenerator` scope. Therefore, the `moduleId` in the template data will always be `rest`. Even if you specify another module in `add` or `update`, it will not be changed.

Only two providers are available for REST:

- `Bitrix\DocumentGenerator\DataProvider\Rest` - must always be specified as the provider for the template;
- `Bitrix\DocumentGenerator\DataProvider\HashDataProvider` - used for passing data into tables / repeating blocks.

The binding of the template to the user is not considered by the REST methods themselves. However, it can be used on the application side.

## Numbering

For working with numberers, there are methods `documentgenerator.numerator.*`, described [here](./numerators/index.md). It should be noted that through this scope, you can access all numberers for documents, including those that work in CRM. However, you can only update/delete the numberer that was created through REST.

## List of Regions

Each template is tied to a specific country. The list of countries is fixed and currently consists of:

- com - Russia
- com - Belarus
- com - Kazakhstan
- com - Ukraine
- com - Brazil
- com - Mexico
- com - Germany
- com - United Kingdom
- com - Poland

Starting from version documentgenerator 18.6.1, it became possible to add your own regions. A [separate section](./region/index.md) was created for managing them.

## Summary

**What can be done?**

- Create documents based on templates in .docx format;
- Insert lists with an arbitrary number of items into the template through tables or repeating blocks;
- Insert images into the template, including from lists;
- Insert fields in HTML format with partial preservation of formatting;
- Create documents, send them, and track views without user involvement (through automation rules).

**What cannot be done?**

- Insert multiple values for the "file" field type;
- Insert tables and images from HTML;
- Insert vector images;
- Formatting transfer is not fully implemented.