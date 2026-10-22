# Inventory Management Vendors: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The vendor directory is the legacy way to store Inventory Management vendors: a vendor in it is a legal entity with a company name. The `catalog.contractor.add` method adds a vendor to the directory, and the `catalog.contractor.field.list` and `catalog.contractor.field.get` methods return the description of vendor fields.

{% note warning "" %}

The `catalog.contractor.add` method works only if Inventory Management vendors have not been moved to CRM. If vendors are managed in CRM, the directory is not used: vendors are added as CRM companies and contacts and bound to stock receipt documents using the [catalog.document.contractor.*](../documentcontractor/index.md) methods.

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Vendors in the Inventory Management](https://helpdesk.bitrix24.com/open/26013559/)

## How to Start

1. Check the available vendor fields using the [catalog.contractor.field.list](./catalog-contractor-field-list.md) method
2. Add a vendor using the [catalog.contractor.add](./catalog-contractor-add.md) method and save the `id` from the response
3. Specify the vendor `id` in the `contractorId` field of a stock receipt document using the [catalog.document.*](../document/index.md) methods

REST 3.0 has no methods for retrieving the list of directory vendors. The identifier of a new vendor is returned by the [catalog.contractor.add](./catalog-contractor-add.md) method.

## Relationships with Other Objects

**Inventory documents.** A vendor from the directory is specified in the `contractorId` field of a stock receipt document. To work with documents, use the [catalog.document.*](../document/index.md) methods.

**CRM vendors.** If vendors have been moved to CRM, CRM companies and contacts are used instead of the directory. You can bind them to a document using the [catalog.document.contractor.*](../documentcontractor/index.md) methods.

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods:
> - a user with the "View and select warehouses" permission — for `catalog.contractor.add`
> - any user — for `catalog.contractor.field.list` and `catalog.contractor.field.get`

#|
|| **Method** | **Description** ||
|| [catalog.contractor.add](./catalog-contractor-add.md) | Adds a vendor to the Inventory Management vendor directory ||
|| [catalog.contractor.field.list](./catalog-contractor-field-list.md) | Returns the list of vendor fields ||
|| [catalog.contractor.field.get](./catalog-contractor-field-get.md) | Returns the description of a vendor field ||
|#
