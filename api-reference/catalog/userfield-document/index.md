# Custom Fields for Inventory Accounting Documents: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This section contains methods for reading and updating the values of custom fields in inventory accounting documents. The creation and configuration of the custom fields themselves are performed in a separate section `userfieldconfig.*`.

> Quick Navigation: [All Methods](#all-methods)
>
> User Documentation: [Getting Started with Inventory Accounting](https://helpdesk.bitrix24.com/open/14836088/)

## Relationship with Other Objects

**Custom Fields.** Fields are created and configured using the methods in the [userfieldconfig.*](../../crm/universal/userfieldconfig/userfieldconfig/index.md) section. For inventory accounting documents, use `moduleId = catalog` and `entityId` in the format `CAT_STORE_DOCUMENT_<documentType>`, for example, `CAT_STORE_DOCUMENT_A`.

**Type of Inventory Accounting Document.** The methods in this section use `documentType`. Acceptable values can be obtained using the [catalog.enum.getStoreDocumentTypes](../enum/catalog-enum-get-store-document-types.md) method.

**Inventory Accounting Document.** To update values, a `documentId` is required. The document identifier can be obtained using the [catalog.document.list](../document/catalog-document-list.md) method.

{% note tip "User Documentation" %}

- [Inventory Accounting in Bitrix24](https://helpdesk.bitrix24.com/open/14821994/)
- [How to Set Access Permissions in CRM for Working with Inventory Accounting Documents](https://helpdesk.bitrix24.com/open/25829011/)
- [How to Create a Receipt Document](https://helpdesk.bitrix24.com/open/22541392/)
- [How to Create an Incoming Document](https://helpdesk.bitrix24.com/open/25801187/)

{% endnote %}

## Working with Custom Fields of Documents

1. Create a custom field using the [userfieldconfig.add](../../crm/universal/userfieldconfig/userfieldconfig/userfieldconfig-add.md) method.
2. Obtain `documentType` via [catalog.enum.getStoreDocumentTypes](../enum/catalog-enum-get-store-document-types.md).
3. Retrieve the current field values using the [catalog.userfield.document.list](./catalog-userfield-document-list.md) method.
4. Update the values using the [catalog.userfield.document.update](./catalog-userfield-document-update.md) method.

## Overview of Methods {#all-methods}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [catalog.userfield.document.update](./catalog-userfield-document-update.md) | Updates the values of custom fields in inventory accounting documents ||
|| [catalog.userfield.document.list](./catalog-userfield-document-list.md) | Returns a list of values for custom fields in inventory accounting documents ||
|#