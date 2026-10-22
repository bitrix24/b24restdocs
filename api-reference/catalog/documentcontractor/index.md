# CRM Vendors: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note info "" %}

CRM vendors are available starting from version 23.0.0 of the Trade Catalog module and REST API 23.100.0.

{% endnote %}

Vendors are individuals and legal entities that supply goods. In CRM, they are added as contacts and companies, but they do not participate in deals, leads, or estimates. CRM vendor data is indicated only in stock receipt documents. Multiple contacts and one company can be attached to a single document.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Vendors](https://helpdesk.bitrix24.com/open/16764476/)

## How to Start

1. Create or find a stock receipt document using the [catalog.document.*](../document/index.md) methods
2. Get the `ID` of a CRM contact or company
3. Add the vendor binding using [catalog.documentcontractor.add](./catalog-documentcontractor-add.md)
4. Check the bindings using [catalog.documentcontractor.list](./catalog-documentcontractor-list.md)

## Relationship with Other Objects

**Inventory Management.** Vendors are indicated in stock receipt documents. To work with documents, use the methods [catalog.document.\*](../document/index.md).

**CRM.** Vendors are CRM entities. To work with them, use the universal methods [crm.item.\*](../../crm/universal/index.md).

## Document Vendors in REST 3.0

In [REST 3.0](../../rest-v3.md), the methods are named `catalog.document.contractor.*`, not `catalog.documentcontractor.*`: the old names at the `/rest/api/` address return the `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION` error. There are methods for adding, retrieving a list, and deleting bindings, as well as methods for retrieving the field schema. Methods available in both API versions are marked `v 3.0` in the table.

Specifics of the new version methods:

- a vendor can be bound only to a stock receipt document `A`: several contacts and one company
- the methods work only if vendors are managed in CRM

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#|
|| **Method** | **Description** ||
|| [catalog.documentcontractor.add](./catalog-documentcontractor-add.md) | Adds a vendor binding to the document ||
|| [catalog.document.contractor.add](./catalog-documentcontractor-add-rest-v3.md) | Adds a vendor binding to the document v 3.0 ||
|| [catalog.documentcontractor.list](./catalog-documentcontractor-list.md) | Returns a list of vendor bindings to documents by filter ||
|| [catalog.document.contractor.list](./catalog-documentcontractor-list-rest-v3.md) | Returns a list of vendor bindings to documents by filter v 3.0 ||
|| [catalog.documentcontractor.delete](./catalog-documentcontractor-delete.md) | Removes the vendor binding from the document by binding ID ||
|| [catalog.document.contractor.delete](./catalog-documentcontractor-delete-rest-v3.md) | Removes the vendor binding from the document by binding ID v 3.0 ||
|| [catalog.documentcontractor.getFields](./catalog-documentcontractor-get-fields.md) | Returns the description of fields for binding a vendor to a document ||
|| [catalog.document.contractor.field.list](./catalog-documentcontractor-field-list.md) | Returns a list of vendor binding fields v 3.0 ||
|| [catalog.document.contractor.field.get](./catalog-documentcontractor-field-get.md) | Returns the description of a vendor binding field ||
|#
