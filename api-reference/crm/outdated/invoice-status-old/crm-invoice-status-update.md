# Update Invoice Status crm.invoice.status.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Universal Methods for Invoices](../../universal/invoice.md).

{% endnote %}

The method `crm.invoice.status.update` returns the status of an invoice by its identifier.

{% note warning %}

Starting from version 19.0.0, it is recommended to use the method [crm.status.update](../../../crm/status/crm-status-update.md).

{% endnote %}

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the invoice status ||
|| **fields***
[`array`](../../../data-types.md) | Set of fields — an array in the form `array("field_to_update"=>"value"[, ...])`, where "field_to_update" can take values returned by the method [crm.invoice.status.fields](./crm-invoice-status-fields.md). 

{% note info %}

To find out the required format for the fields, execute the method [crm.invoice.status.fields](./crm-invoice-status-fields.md) and check the format of the returned values for these fields.

{% endnote %}
||
|#