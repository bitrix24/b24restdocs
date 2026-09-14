# Update the process rpa.type.update

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

This method updates the process by its `id`.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../data-types.md) | Identifier of the process ||
|| **fields** 
[`array`](../../../data-types.md) | List of process fields. Fully analogous to the set from the [rpa.type.add](./rpa-type-add.md) method ||
|#

## Response Handling

HTTP status: **200**

Returns a response similar to the [rpa.type.get](./rpa-type-get.md) method.

## Continue Your Exploration 

- [{#T}](./index.md)
- [{#T}](./rpa-type-add.md)
- [{#T}](./rpa-type-get.md)
- [{#T}](./rpa-type-list.md)
- [{#T}](./rpa-type-delete.md)