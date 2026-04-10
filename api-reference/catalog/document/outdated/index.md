# Documents: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "" %}

**DEPRECATED**

The development of methods in this section has been halted.  
Please use the section [Warehouse Accounting Documents (`catalog.document.*`)](../index.md).

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)  
> Who can execute the methods: administrator

#|
|| **Method** | **Description** ||
|| [catalog.document.confirm](./catalog-document-confirm.md) | Confirms the warehouse accounting document ||
|| [catalog.document.unconfirm](./catalog-document-unconfirm.md) | Cancels the confirmation of the document ||
|| [catalog.document.fields](./catalog-document-fields.md) | Returns a list of document fields ||
|| [catalog.document.element.fields](./catalog-document-element-fields.md) | Returns a list of item fields in the warehouse accounting document ||
|#