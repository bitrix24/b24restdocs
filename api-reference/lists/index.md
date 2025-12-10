# Universal Lists: Overview of Methods

Universal lists are a tool for creating and managing structured information. They allow you to create customizable tables with various types of fields: text, number, date, file, and others.

Universal lists integrate with Bitrix24 modules, enabling data processing automation and access management. Imagine a list of employee vacation requests. A workflow is set up for the list, and access permissions are configured so that only the manager and HR department staff can modify the requests. As a result, when an employee submits a vacation request, the HR manager automatically receives a notification to review and approve or decline the request.

Each list represents an [information block](*iblock). Use the group of methods [lists.*](./lists/index.md) to work with lists.

> Quick navigation: [all methods](#all-methods) 
>
> User documentation: [Create and configure a list](https://helpdesk.bitrix24.com/open/18128130/)

## Structure of a Universal List

### Sections

Sections group items like folders. This helps establish a hierarchy. For example, the section "Legal Documents" -> subsection "Lease Agreements." To work with sections, use the methods [lists.section.*](./sections/index.md).

### Items

Items are the rows of the list that store the main information. For example, the data of a contract, request, or certificate. An item can be placed in a section. Methods for working with items are [lists.element.*](./elements/index.md).

{% note tip "User Documentation" %}

- [Enable workflows in Lists](https://helpdesk.bitrix24.com/open/8440151/)

{% endnote %}

### Fields

Fields define what data is stored in each item. You specify the field type: text, list, date, file, and others. You can create, modify, and delete fields through the methods [lists.field.*](./fields/index.md).

The set of fields is unique for each list. All available types can be found in the description of the `FIELDS` parameter of the method [lists.field.add](./fields/lists-field-add.md).

Information in universal lists can be linked to CRM and Drive through fields of the following types:

-  Link to CRM entities,
-  File (Drive).

Read about the specifics of these types in the article [Universal List Fields: Overview of Methods](./fields/index.md).

{% note tip "User Documentation" %}

-  [Customize List fields](https://helpdesk.bitrix24.com/open/16702950/)

{% endnote %}

## Sequence of Working with a List

The recommended order of working with a list:

1. Create a list using the method [lists.add](./lists/lists-add.md). Specify the code `IBLOCK_CODE`, type `IBLOCK_TYPE_ID`, and name `NAME` of the list.

2. Add custom fields through the method [lists.field.add](./fields/lists-field-add.md) to define the data structure of future items.

3. If necessary, create sections using the method [lists.section.add](./sections/lists-section-add.md) to organize the hierarchy.

4. Populate the list with items using the method [lists.element.add](./elements/lists-element-add.md). Fill in the created fields and link items to sections.

## Overview of Methods {#all-methods}

> Scope: [`lists`](../scopes/permissions.md)
>
> Who can execute the method: depends on the method

### Lists

#|
|| **Method** | **Description** ||
|| [lists.add](./lists/lists-add.md) | Creates a universal list ||
|| [lists.update](./lists/lists-update.md) | Updates a universal list ||
|| [lists.get](./lists/lists-get.md) | Returns data of a universal list or an array of lists ||
|| [lists.delete](./lists/lists-delete.md) | Deletes a universal list ||
|| [lists.get.iblock.type.id](./lists/lists-get-iblock-type-id.md) | Returns the identifier of the information block type ||
|#

### Items

#|
|| **Method** | **Description** ||
|| [lists.element.add](./elements/lists-element-add.md) | Creates a list item ||
|| [lists.element.update](./elements/lists-element-update.md) | Updates a list item ||
|| [lists.element.get](./elements/lists-element-get.md) | Returns an item or a list of items ||
|| [lists.element.delete](./elements/lists-element-delete.md) | Deletes a list item ||
|| [lists.element.get.file.url](./elements/lists-element-get-file-url.md) | Returns the file path ||
|#

### Fields

#|
|| **Method** | **Description** ||
|| [lists.field.add](./fields/lists-field-add.md) | Creates a list field ||
|| [lists.field.update](./fields/lists-field-update.md) | Updates a list field ||
|| [lists.field.get](./fields/lists-field-get.md) | Returns field data ||
|| [lists.field.delete](./fields/lists-field-delete.md) | Deletes a list field ||
|| [lists.field.type.get](./fields/lists-field-type-get.md) | Returns available field types for the specified list ||
|#

### Sections

#|
|| **Method** | **Description** ||
|| [lists.section.add](./sections/lists-section-add.md) | Creates a list section ||
|| [lists.section.update](./sections/lists-section-update.md) | Updates a list section ||
|| [lists.section.get](./sections/lists-section-get.md) | Returns a section or a list of sections ||
|| [lists.section.delete](./sections/lists-section-delete.md) | Deletes a list section ||
|#

[*iblock]: An information block is a special object for storing news, services, articles, product catalogs, and other data with a clear structure.