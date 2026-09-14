# Bottom Dropdown Menu

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Items in the bottom dropdown menu of the [timeline entry](../index.md) `MenuItemDto`.

## Parameters of the `MenuItemDto` Object

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **title^*^**
[`textWithTranslation`](./field-types.md#textwithtranslation) | Button text ||
|| **action^*^**
[`ActionDto`](./action.md) | Action to be performed when the button is pressed ||
|| **scope**
[`string`](../../../../../data-types.md) | [Scope](./field-types.md#scope), for example `web` ||
|| **hideIfReadonly**
[`boolean`](../../../../../data-types.md) | Flag. Hides the tag if the user does not have edit access, default is `false` ||
|#

## Example

```json
{
    "title": "Confirm Process",
    "action": {
        "type": "restEvent",
        "id": "confirmProcess",
        "animationType": "loader"
    },
    "scope": "web",
    "hideIfReadonly": true
}
```

## Continue Exploring

- [{#T}](./layout.md)
- [{#T}](./icon.md)
- [{#T}](./body.md)
- [{#T}](./content-block.md)
- [{#T}](./header.md)
- [{#T}](./footer.md)
- [{#T}](./action.md)
- [{#T}](./field-types.md)
- [{#T}](./rest-app-layout-dto.md)
- [{#T}](./examples.md)