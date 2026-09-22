# Main Content Area of Configurable Activity

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

`BodyDto` is the main content area of the [timeline entry](../index.md): a logo and a set of content blocks that make up the entry content. The object is passed in the `body` field of the [configurable activity structure](./layout.md) when calling the methods [crm.activity.configurable.add](../crm-activity-configurable-add.md) and [crm.activity.configurable.update](../crm-activity-configurable-update.md).

Block types and their fields are described on the [content block](./content-block.md) page. For ready-to-use combinations — an entry with a set of fields, different action types, and multiple languages — see [Activity Configuration Examples](./examples.md).

## Parameters of the `BodyDto` Object

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **logo^*^**
[`LogoDto`](#logo-dto) | Logo of the entry ||
|| **blocks^*^**
[`object`](../../../../../data-types.md) | Content blocks of the entry: the key is a block identifier that you define yourself, and the value is a [ContentBlockDto](./content-block.md) object. The key may contain Latin letters, digits, hyphens, and underscores. Pass at least one block, but no more than 20 ||
|#

If the structure violates these restrictions, the method returns a validation error. Error codes are listed on the pages [crm.activity.configurable.add](../crm-activity-configurable-add.md#errors) and [crm.activity.configurable.update](../crm-activity-configurable-update.md#errors).

## `LogoDto` Object {#logo-dto}

Logo of the timeline entry.

### Parameters of the `LogoDto` Object

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **code^*^**
[`string`](../../../../../data-types.md) | Logo code, for example `call-incoming` or `notification`. All available codes are returned by the [crm.timeline.logo.list](../../../logmessage/logo/crm-timeline-logo-list.md) method. A custom logo is added by the [crm.timeline.logo.add](../../../logmessage/logo/crm-timeline-logo-add.md) method ||
|| **action**
[`ActionDto`](./action.md) | Action to be taken when the logo is clicked ||
|#

## Example Object

The value of the `body` field: an incoming call logo with a link to a deal and one text block.

```json
{
    "logo": {
        "code": "call-incoming",
        "action": {
            "type": "redirect",
            "uri": "/crm/deal/details/123/"
        }
    },
    "blocks": {
        "text": {
            "type": "text",
            "properties": {
                "value": "The client confirmed the meeting"
            }
        }
    }
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
