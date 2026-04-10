# Remove Automation Rule from Process rpa.task.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Smart scripts](../../../crm/universal/user-defined-object-types/index.md) as an alternative to this functionality.

{% endnote %}

This method removes the Automation rule named `robotName` from the process with the identifier `typeId` at the stage with the identifier `stageId`.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **typeId** 
[`integer`](../../../data-types.md) | Identifier of the process ||
|| **stageId** 
[`integer`](../../../data-types.md) | Identifier of the stage ||
|| **robotName** 
[`string`](../../../data-types.md) | Name of the Automation rule ||
|#

## Continue Exploring 

- [{#T}](./index.md)
- [{#T}](./rpa-task-add-user.md)
- [{#T}](./rpa-task-do.md)