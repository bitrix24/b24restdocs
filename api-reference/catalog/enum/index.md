# Enumeration of the Trade Catalog: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `catalog.enum.*` methods return reference enumerations from the Trade Catalog for use in other methods.

> Quick Navigation: [all methods](#all-methods)

## How to Start

1. Determine which scenario needs a reference list: price rounding or inventory documents
2. Get the list of allowed values using a `catalog.enum.*` method
3. Pass the returned code to the related Trade Catalog method

## Where Catalog Enumerations Are Used

**Price Rounding.** The method [catalog.enum.getRoundTypes](./catalog-enum-get-round-types.md) returns codes for rounding types. These values are used when configuring rounding rules through the methods [catalog.roundingRule.*](../rounding-rule/index.md).

**Warehouse Documents.** The method [catalog.enum.getStoreDocumentTypes](./catalog-enum-get-store-document-types.md) returns types of warehouse accounting documents. These values are utilized in the methods of the [catalog.document.*](../document/index.md) section when creating and processing documents.

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the methods: any user

#|
|| **Method** | **Description** ||
|| [catalog.enum.getRoundTypes](./catalog-enum-get-round-types.md) | Returns a list of rounding types available in the catalog ||
|| [catalog.enum.getStoreDocumentTypes](./catalog-enum-get-store-document-types.md) | Returns types of warehouse accounting documents available for REST ||
|#
