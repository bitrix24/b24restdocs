# Event for Adding Custom Field onCrmInvoiceUserFieldAdd

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The event is triggered when a custom field is added.

## Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../../data-types.md)| Identifier of the custom field ||
|| **entityId** 
[`string`](../../../../data-types.md)| Symbolic identifier of the entity for which the field was created ||
|| **fieldName** 
[`string`](../../../../data-types.md)| Name of the created custom field ||
|#