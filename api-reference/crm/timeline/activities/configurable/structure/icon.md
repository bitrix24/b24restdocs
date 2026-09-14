# Icon

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Icon of the [timeline entry](../index.md) `IconDto`.

## Parameters of the `IconDto` Object

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** | **Additional** ||
|| **code^*^**
[`string`](../../../../../data-types.md) | Icon code | A list of available codes can be obtained using the [crm.timeline.icon.list](../../../logmessage/icons/crm-timeline-icon-list.md) method ||
|#

## Example of the Object

```json
{
    "icon": {
        "code": "call-completed"
    }
}
```

## Continue Exploring

- [{#T}](./layout.md)
- [{#T}](./header.md)
- [{#T}](./body.md)
- [{#T}](./content-block.md)
- [{#T}](./footer.md)
- [{#T}](./menu-item.md)
- [{#T}](./action.md)
- [{#T}](./field-types.md)
- [{#T}](./rest-app-layout-dto.md)
- [{#T}](./examples.md)