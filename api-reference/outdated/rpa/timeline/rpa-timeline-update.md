# Update Timeline Entry rpa.timeline.update

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

This method updates the timeline entry with the identifier `id`.

The method only updates the `title` and `description` fields, and only for entries created by the same user and through the application.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../data-types.md) | Identifier of the entry ||
|| **fields** 
[`object`](../../../data-types.md) | Object containing the [fields](#fields) of the entry ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **title** 
[`string`](../../../data-types.md) | Title of the entry ||
|| **description** 
[`string`](../../../data-types.md) | Description of the entry. HTML tags can be used ||
|#

## Continue Exploring 

- [{#T}](./index.md)
- [{#T}](./rpa-timeline-add.md)
- [{#T}](./rpa-timeline-update-is-fixed.md)
- [{#T}](./rpa-timeline-list-for-item.md)
- [{#T}](./rpa-timeline-delete.md)