# Finding and Processing Duplicates in CRM: Overview of Methods

A duplicate is a set of CRM elements that stand for one and the same person or organization. The first call from a customer creates a contact with a name and phone number, and an email from that same customer creates a second contact with a name and email address. Bitrix24 can find such pairs and merge them into a single element.

Duplicate search works only with leads, contacts, and companies — both in the interface and through the method [crm.duplicate.findbycomm](./crm-duplicate-find-by-comm.md). Merging works more broadly: the method [crm.entity.mergeBatch](./crm-entity-merge-batch.md) also accepts deals, invoices, estimates, and SPA elements, provided that you have determined the identifiers of the elements to merge yourself.

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Finding and processing duplicates in Bitrix24](https://helpdesk.bitrix24.com/open/18346126/)

## How to Choose a Method

#|
|| **If You Need To** | **Use** ||
|| Check, before creating a lead, contact, or company, whether such a phone number or email address is already in the database | [crm.duplicate.findbycomm](./crm-duplicate-find-by-comm.md) ||
|| Merge several elements of the same type into one | [crm.entity.mergeBatch](./crm-entity-merge-batch.md) ||
|| Add a new field to the duplicate search in the interface, for example a Tax ID or a custom field | The methods of the [Duplicate Search Settings for Additional Fields](./volatile-type/index.md) section ||
|#

## Searching for Duplicates via REST

The method [crm.duplicate.findbycomm](./crm-duplicate-find-by-comm.md) searches for matches by phone number or email address. The additional field settings do not affect it.

{% note tip "Tutorial on using the method" %}

- [How to Find Duplicates in CRM by Phone and Email](../../../tutorials/crm/how-to-get-lists/search-by-phone-and-email.md)

{% endnote %}

## Merging Duplicates

The method [crm.entity.mergeBatch](./crm-entity-merge-batch.md) merges elements of the same type. The first existing element in the `entityIds` array becomes the main one. The data of the other elements found is transferred into it. How identifiers are handled and the merge conditions are described on the method page.

## Setting Up Duplicate Search in the Interface

The set of fields used to search for duplicates in the Bitrix24 interface can be extended with the `crm.duplicate.volatileType.*` methods. The available fields and limitations are described in the section [Duplicate Search Settings for Additional Fields](./volatile-type/index.md).

## Overview of Methods {#all-methods}

### Searching and Merging Duplicates

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the methods: depends on the method — searching requires permission to read leads, contacts, or companies, merging requires permission to read and modify the main element and to read and delete the others

#|
|| **Method** | **Description** ||
|| [crm.duplicate.findbycomm](./crm-duplicate-find-by-comm.md) | Returns a list of duplicates ||
|| [crm.entity.mergeBatch](./crm-entity-merge-batch.md) | Merges duplicates ||
|#

### Setting Up Duplicate Search

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the methods: Bitrix24 administrator or CRM administrator

#|
|| **Method** | **Description** ||
|| [crm.duplicate.volatileType.fields](./volatile-type/crm-duplicate-volatile-type-fields.md) | Returns a list of fields that can be used to search for duplicates ||
|| [crm.duplicate.volatileType.list](./volatile-type/crm-duplicate-volatile-type-list.md) | Returns a list of fields connected to duplicate search ||
|| [crm.duplicate.volatileType.register](./volatile-type/crm-duplicate-volatile-type-register.md) | Adds a field to the duplicate search ||
|| [crm.duplicate.volatileType.unregister](./volatile-type/crm-duplicate-volatile-type-unregister.md) | Removes a field from the duplicate search ||
|#
