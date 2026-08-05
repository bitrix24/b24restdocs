# CRM Vendors: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

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

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods: depending on the method

#|
|| **Method** | **Description** ||
|| [catalog.documentcontractor.add](./catalog-documentcontractor-add.md) | Adds a vendor binding to the document ||
|| [catalog.documentcontractor.list](./catalog-documentcontractor-list.md) | Returns a list of vendor bindings to documents by filter ||
|| [catalog.documentcontractor.delete](./catalog-documentcontractor-delete.md) | Removes the vendor binding from the document by binding ID ||
|| [catalog.documentcontractor.getFields](./catalog-documentcontractor-get-fields.md) | Returns the description of fields for binding a vendor to a document ||
|#
