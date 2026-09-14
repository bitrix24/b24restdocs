# Delete Comment rpa.comment.delete

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

This method deletes a comment with the identifier `id`.

It only allows the deletion of comments that were added by the same user.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../../data-types.md) | Record identifier ||
|#

## Continue Your Exploration 

- [{#T}](./index.md)
- [{#T}](./rpa-comment-add.md)
- [{#T}](./rpa-comment-update.md)