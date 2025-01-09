# About the Document Generator

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

Currently, there are **two scopes** for working with the document generator:
- Methods `crm.documentgenerator.*`. The results of these methods are displayed in the CRM interface;
- Methods `documentgenerator.*`. The results of these methods are only available at the REST level.

You cannot access data from one scope using methods from another:
- You cannot create a CRM document with a template for REST;
- You cannot use CRM data when working with methods `documentgenerator.*`.

The following **field types** and their **modifiers** are supported:

- IMAGE - images;
- STAMP - stamps and signatures;
- DATE - dates;
- NAME - names.

The field types **Money** and **Address** are implemented within the *crm* module, so they cannot be used in the REST of this module. If you need to output such data, you will have to pass it in a pre-formed format.

It is possible to use arrays for inserting into tables and repeating blocks.

## Differences in Method Parameters Across Scopes

“From the inside,” the methods are identical. In fact, the methods `crm.documentgenerator.*` call the methods `documentgenerator.*` after preprocessing the parameters. However, there are several differences:
- For the input of methods `crm.documentgenerator.*`, you need to pass the **ID** of the CRM entity type instead of provider names (parameter `entityTypeId`);
- For the input of methods `crm.documentgenerator.*`, you need to pass the parameter `entityId` - **ID** of the CRM entity instead of the **value** parameter.

## Templates

All templates and documents created by this API are tied to the REST module. You cannot access templates and documents from other modules through the `documentgenerator` scope. Therefore, the `moduleId` in the template data will always be `rest`. Even if you specify another module in `add` or `update`, it will not be changed.

Only two providers are available for REST:

- `Bitrix\DocumentGenerator\DataProvider\Rest` - must always be specified as the provider for the template
- `Bitrix\DocumentGenerator\DataProvider\HashDataProvider` - used for passing data into tables / repeating blocks

The binding of the template to the user is not considered by the REST methods themselves. However, it can be used on the application side.

## Enumerators

To work with enumerators, there are methods `documentgenerator.numerator.*`, described [here](./numerators/index.md). It should be noted that through this scope, you can access all enumerators for documents, including those that work in CRM. However, you can only update/delete the enumerator that was created through REST.

## List of Regions

Each template is tied to a specific country. The list of countries is fixed and currently consists of:

- de - Germany
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
- Create documents, send them, and track views without user involvement (through automation rules).

**What cannot be done?**

- Insert multiple values for the "file" field type;
- Insert tables and images from HTML;
- Insert vector images;
- Formatting transfer is not fully executed.