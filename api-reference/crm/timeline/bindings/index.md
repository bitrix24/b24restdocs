# Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

Content is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [crm.timeline.bindings.bind](./crm-timeline-bindings-bind.md) | Adds a link between a timeline record and a CRM entity ||
|| [crm.timeline.bindings.list](./crm-timeline-bindings-list.md) | Retrieves a list of links for a timeline record ||
|| [crm.timeline.bindings.unbind](./crm-timeline-bindings-unbind.md) | Removes a link between a timeline record and a CRM entity ||
|| [crm.timeline.bindings.fields](./crm-timeline-bindings-fields.md) | Retrieves the fields of the link between CRM entities and the timeline record ||
|#