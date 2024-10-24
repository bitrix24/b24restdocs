# Universal Lists: Overview of Methods

Universal lists are a tool for creating and managing structured information. They allow for the creation of customizable tables with various types of fields: text, number, date, file, and others.

Universal lists integrate with *Bitrix24* modules, enabling the automation of data processing and access management. Imagine a list of employee vacation requests. A workflow is set up for the list, and access permissions are configured so that only the manager and HR department employees can modify the requests. As a result, when an employee submits a vacation request, the HR manager automatically receives a notification to review and approve or deny the request.

Each list represents an [information block](*iblock). Use the group of methods [lists.*](./lists/index.md) to work with lists.

> Quick navigation: [all methods](#all-methods) 
>
> User documentation: [lists in Bitrix24](https://helpdesk.bitrix24.com/open/18128130/)

## Structure of Lists

**Sections**. Used for grouping information and building a convenient hierarchy. Section management is performed through the methods [lists.section.*](./sections/index.md).

**Elements**. Store the main information. Managed by the group of methods [lists.element.*](./elements/index.md).

## Workflows

Workflows in universal lists create a custom scenario for processing elements. Workflow management is carried out using the groups of methods [bizproc.workflow.*](../bizproc/index.md) and [bizproc.task.*](../bizproc/bizproc-task/bizproc-task-list.md).

{% note tip "User Documentation" %}

- [Enabling workflows in the list](https://helpdesk.bitrix24.com/open/8440151/)

{% endnote %}

## Fields

Element fields are unique to each list. Fields can be created, modified, and deleted using the group of methods [lists.field.*](./fields/index.md).

The `TYPE` parameter defines the data type of the field. The list of available types is provided in the description of the `FIELDS` parameter of the method [lists.field.add](./fields/lists-field-add.md). 

Information in universal lists can be linked to CRM and Drive through fields of the following types:
- Link to CRM elements (`S:ECrm`)
- File (Drive) (`S:DiskFile`)

Read about the specifics of these types in the article [Fields: Overview of Methods](./fields/index.md).

## Working Groups and Projects

Universal lists are integrated [into working groups and projects](../sonet-group/sonet-group-create.md). You can create your lists within any group or project with various levels of access for participants.

The identifier for the information block type `IBLOCK_TYPE_ID` for group lists takes the value `lists_socnet`.

## Overview of Methods

> Scope: [`lists`](../scopes/permissions.md)
>
> Who can execute the method: any user

### Lists

#| 
|| **Method** | **Description** ||
|| [lists.add](./lists/lists-add.md) | Creates a list ||
|| [lists.delete](./lists/lists-delete.md) | Deletes a list ||
|| [lists.get](./lists/lists-get.md) | Returns data on lists ||
|| [lists.update](./lists/lists-update.md) | Updates an existing list ||
|| [lists.get.iblock.type.id](./lists/lists-get-iblock-type-id.md) | Returns the identifier of the information block type ||
|#

### Elements

#| 
|| **Method** | **Description** ||
|| [lists.element.add](./elements/lists-element-add.md) | Creates a list element ||
|| [lists.element.delete](./elements/lists-element-delete.md) | Deletes a list element ||
|| [lists.element.get](./elements/lists-element-get.md) | Returns a list of elements or an element ||
|| [lists.element.update](./elements/lists-element-update.md) | Updates a list element ||
|| [lists.element.get.file.url](./elements/lists-element-get-file-url.md) | Returns the file path ||
|#

### Fields

#| 
|| **Method** | **Description** ||
|| [lists.field.add](./fields/lists-field-add.md) | Creates a list field ||
|| [lists.field.delete](./fields/lists-field-delete.md) | Deletes a list field ||
|| [lists.field.get](./fields/lists-field-get.md) | Returns field data ||
|| [lists.field.type.get](./fields/lists-field-type-get.md) | Returns available field types for the specified list ||
|| [lists.field.update](./fields/lists-field-update.md) | Updates a list field ||
|#

### Sections

#| 
|| **Method** | **Description** ||
|| [lists.section.add](./sections/lists-section-add.md) | Creates a list section ||
|| [lists.section.get](./sections/lists-section-get.md) | Returns data about sections ||
|| [lists.section.update](./sections/lists-section-update.md) | Updates a list section ||
|| [lists.section.delete](./sections/lists-section-delete.md) | Deletes a list section ||
|#

[*iblock]: An information block is a special object for storing news, services, articles, product catalogs, and other data with a clear structure.