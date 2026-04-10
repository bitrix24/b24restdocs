# Numbering Systems

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- An introduction corresponding to the title is needed.

{% endnote %}

{% endif %}

## Overview of Methods {#all-methods}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for document generator templates.

#|
|| **Method** | **Description** ||
|| [crm.documentgenerator.numerator.add](./crm-document-generator-numerator-add.md) | Adds a new numbering system ||
|| [crm.documentgenerator.numerator.update](./crm-document-generator-numerator-update.md) | Updates an existing numbering system ||
|| [crm.documentgenerator.numerator.get](./crm-document-generator-numerator-get.md) | Returns information about the numbering system by ID ||
|| [crm.documentgenerator.numerator.list](./crm-document-generator-numerator-list.md) | Returns a list of numbering systems ||
|| [crm.documentgenerator.numerator.delete](./crm-document-generator-numerator-delete.md) | Deletes a numbering system ||
|#