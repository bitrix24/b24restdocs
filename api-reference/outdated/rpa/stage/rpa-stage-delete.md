# Delete Stage rpa.stage.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Smart scripts](../../../crm/universal/user-defined-object-types/index.md) as an alternative to this functionality.

{% endnote %}

This method deletes a stage.

A process must always have one successful stage. It is not possible to delete a successful stage.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`number`](../../../data-types.md) | Identifier of the stage ||
|#

## Continue Your Exploration

- [{#T}](./index.md)
- [{#T}](./rpa-stage-add.md)
- [{#T}](./rpa-stage-update.md)
- [{#T}](./rpa-stage-get.md)
- [{#T}](./rpa-stage-list-for-type.md)