# Document Templates

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- An introduction corresponding to the title is needed.

{% endnote %}

{% endif %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can perform the method: a user with the "edit" access permission for document generator templates.

#|
|| **Method** | **Description** ||
|| [crm.documentgenerator.template.add](./crm-document-generator-template-add.md) | Adds a new template ||
|| [crm.documentgenerator.template.update](./crm-document-generator-template-update.md) | Updates an existing template ||
|| [crm.documentgenerator.template.get](./crm-document-generator-template-get.md) | Returns information about a template by ID ||
|| [crm.documentgenerator.template.list](./crm-document-generator-template-list.md) | Returns a list of templates ||
|| [crm.documentgenerator.template.delete](./crm-document-generator-template-delete.md) | Deletes a template ||
|| [crm.documentgenerator.template.getfields](./crm-document-generator-template-get-fields.md) | Returns the fields of a template ||
|#