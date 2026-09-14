# Create a New Invoice Status crm.invoice.status.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

Development of this method has been halted. Please use [Universal Methods for Invoices](../../universal/invoice.md).

{% endnote %}

The method `crm.invoice.status.add` creates a new invoice status.

{% note warning %}

Starting from version 19.0.0, it is recommended to use the method [crm.status.add](../../../crm/status/crm-status-add.md).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`array`](../../../data-types.md) | A set of fields — an array in the form `array("field"=>"value"[, ...])`, containing the values of the invoice status fields. 

{% note info %}

To find out the required format for the fields, execute the method [crm.invoice.status.fields](./crm-invoice-status-fields.md) and check the format of the returned values for these fields.

{% endnote %}

||
|#