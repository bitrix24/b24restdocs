# Get a List of Invoice Statuses crm.invoice.status.list

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
{% note warning %}

Starting from version 19.0.0, it is recommended to use the method [crm.status.list](../../../crm/status/crm-status-list.md)

{% endnote %}

The method `crm.invoice.status.list` returns a list of invoice statuses based on a filter. It is an implementation of list methods for invoice statuses.

## Method Parameters

See the description of [list methods](../../../../settings/how-to-call-rest-api/list-methods-pecularities.md)