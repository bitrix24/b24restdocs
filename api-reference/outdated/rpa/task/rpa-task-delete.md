# Remove Automation Rule from Process rpa.task.delete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

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