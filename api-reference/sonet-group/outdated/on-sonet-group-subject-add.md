# Event on creating a workgroup topic onSonetGroupSubjectAdd

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onSonetGroupSubjectAdd` is triggered after a workgroup/project topic is created.

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the created topic ||
|#
{% include [Note on required parameters](../../../_includes/required.md) %}