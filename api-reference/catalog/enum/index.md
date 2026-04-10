# Enumeration of the Trade Catalog: Overview of Methods

The methods in this section return reference enumerations from the Trade Catalog for use in other methods.

> Quick Navigation: [all methods](#all-methods)

## Where Catalog Enumerations Are Used

**Price Rounding.** The method [catalog.enum.getRoundTypes](./catalog-enum-get-round-types.md) returns codes for rounding types. These values are used when configuring rounding rules through the methods [catalog.roundingRule.*](../rounding-rule/index.md).

**Warehouse Documents.** The method [catalog.enum.getStoreDocumentTypes](./catalog-enum-get-store-document-types.md) returns types of warehouse accounting documents. These values are utilized in the methods of the [catalog.document.*](../document/index.md) section when creating and processing documents.

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#| 
|| **Method** | **Description** ||
|| [catalog.enum.getRoundTypes](./catalog-enum-get-round-types.md) | Returns a list of rounding types available in the catalog ||
|| [catalog.enum.getStoreDocumentTypes](./catalog-enum-get-store-document-types.md) | Returns types of warehouse accounting documents available for REST ||
|#