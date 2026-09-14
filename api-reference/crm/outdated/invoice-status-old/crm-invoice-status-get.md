# Get Invoice Status by ID crm.invoice.status.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute this method: any user

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [Universal Methods for Invoices](../../universal/invoice.md).

{% endnote %}

The method `crm.invoice.status.get` returns the status of an invoice based on its ID.

{% note warning %}

Starting from version 19.0.0, it is recommended to use the method [crm.status.get](../../../crm/status/crm-status-get.md).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#| 
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../../data-types.md) | The identifier of the invoice status || 
|#