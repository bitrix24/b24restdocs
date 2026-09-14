# Event for Changing Workgroup Subject onSonetGroupSubjectUpdate

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sonet`](../../scopes/permissions.md)
>
> Who can subscribe: any user

The event `onSonetGroupSubjectUpdate` is triggered after the subject of a workgroup/project is changed.

#|
|| **Field** | **Description** ||
|| **ID** | Identifier of the changed subject ||
|#
{% include [Note on required parameters](../../../_includes/required.md) %}