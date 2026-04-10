# Get a List of Invoice Statuses crm.invoice.status.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

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