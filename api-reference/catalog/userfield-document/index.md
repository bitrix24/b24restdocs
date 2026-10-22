# Custom Fields for Inventory Accounting Documents: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The `catalog.userfield.document.*` methods read and update the values of custom fields for inventory accounting documents. Custom fields themselves are created and configured using `userfieldconfig.*` methods.

> Quick navigation: [all methods](#all-methods)

## Relationships with Other Objects

**Custom Fields.** Fields are created and configured using [userfieldconfig.*](../../crm/universal/userfieldconfig/index.md) methods. For inventory accounting documents, use `moduleId = catalog` and `entityId` in the format `CAT_STORE_DOCUMENT_<documentType>`, for example, `CAT_STORE_DOCUMENT_A`.

**Type of Inventory Accounting Document.** The methods in this section use `documentType`. Acceptable values can be obtained using the [catalog.enum.getStoreDocumentTypes](../enum/catalog-enum-get-store-document-types.md) method.

**Inventory Accounting Document.** To update values, a `documentId` is required. The document identifier can be obtained using the [catalog.document.list](../document/catalog-document-list.md) method.

{% note tip "User Documentation" %}

- [Bitrix24 Inventory Management ](https://helpdesk.bitrix24.com/open/14821994/)
- [Access permissions for Inventory management documents](https://helpdesk.bitrix24.com/open/25829011/)
- [Create a stock adjustment](https://helpdesk.bitrix24.com/open/22541392/)
- [Create a stock receipt](https://helpdesk.bitrix24.com/open/25801187/)

{% endnote %}

## How to Start

1. Create a custom field using [userfieldconfig.add](../../crm/universal/userfieldconfig/userfieldconfig-add.md)
2. Retrieve the `documentType` using [catalog.enum.getStoreDocumentTypes](../enum/catalog-enum-get-store-document-types.md)
3. Get the current field values using [catalog.userfield.document.list](./catalog-userfield-document-list.md)
4. Update the values using [catalog.userfield.document.update](./catalog-userfield-document-update.md)

## Custom Fields in REST 3.0

In [REST 3.0](../../rest-v3.md), the methods are named `catalog.document.userfield.*`, not `catalog.userfield.document.*`: the old names at the `/rest/api/` address return the `BITRIX_REST_V3_EXCEPTION_METHODNOTFOUNDEXCEPTION` error. There are methods for retrieving values, updating values, and retrieving the field schema. Methods available in both API versions are marked `v 3.0` in the table.

Specifics of the new version methods:

- the [catalog.document.userfield.list](./catalog-userfield-document-list-rest-v3.md) method requires a condition on `docType` with the `=` operator in `filter`
- the [catalog.document.userfield.update](./catalog-userfield-document-update-rest-v3.md) method requires the type of the document itself in `fields.docType`
- in the `list` response, value keys are `UF_*`; in `update`, a value can be passed by the `UF_*` or `FIELD_{id}` key

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#|
|| **Method** | **Description** ||
|| [catalog.userfield.document.list](./catalog-userfield-document-list.md) | Returns a list of values for custom fields of inventory accounting documents ||
|| [catalog.document.userfield.list](./catalog-userfield-document-list-rest-v3.md) | Returns a list of values for custom fields of inventory accounting documents v 3.0 ||
|| [catalog.document.userfield.field.list](./catalog-userfield-document-field-list.md) | Returns a list of fields for document custom fields v 3.0 ||
|| [catalog.document.userfield.field.get](./catalog-userfield-document-field-get.md) | Returns the description of a field for document custom fields ||
|| [catalog.userfield.document.update](./catalog-userfield-document-update.md) | Updates the values of custom fields for inventory accounting documents ||
|| [catalog.document.userfield.update](./catalog-userfield-document-update-rest-v3.md) | Updates the values of custom fields for inventory accounting documents v 3.0 ||
|#
