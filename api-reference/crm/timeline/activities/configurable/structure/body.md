# Main Content Area of Configurable Activity

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

`BodyDto` is the main content area of the [timeline entry](../index.md).

## Parameters of the `BodyDto` Object

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **logo^*^**
[`LogoDto`](#obuekt) | An object describing the logo of the timeline entry ||
|| **blocks**
[`ContentBlockDto`](./content-block.md) | An associative array of objects describing content blocks 

{% note warning %}

The array must contain at least one element and no more than 20 elements.

{% endnote %}

||
|#

## `LogoDto` Object {#obuekt}

Logo of the timeline entry.

### Parameters of the `LogoDto` Object

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **code^*^**
[`string`](../../../../../data-types.md) | Logo code, for example `call`. A list of available codes can be obtained using the [crm.timeline.logo.list](../../../logmessage/logo/crm-timeline-logo-list.md) method ||
|| **action**
[`ActionDto`](./action.md) | Action to be taken when the logo is clicked ||
|#

## Example Object (Without Content Blocks)

```json
{
    "body": {
        "logo": {
            "code": "call-incoming",
            "action": {
                "type": "redirect",
                "uri": "/crm/deal/details/123/"
            }
        },
        "blocks": {

        }
    },
}
```

## Continue Learning

- [{#T}](./layout.md)
- [{#T}](./header.md)
- [{#T}](./icon.md)
- [{#T}](./content-block.md)
- [{#T}](./footer.md)
- [{#T}](./menu-item.md)
- [{#T}](./action.md)
- [{#T}](./field-types.md)
- [{#T}](./rest-app-layout-dto.md)
- [{#T}](./examples.md)