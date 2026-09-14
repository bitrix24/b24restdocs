# Documents: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

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