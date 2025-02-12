# Main Content Area of Configurable Deal

`BodyDto` is the main content area of the [timeline entry](../index.md).

## Parameters of the `BodyDto` Object

{% include [Footnote on parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **logo^*^**
[`LogoDto`](#object) | An object describing the logo of the timeline entry ||
|| **blocks**
[`ContentBlockDto`](./content-block.md) | An associative array of objects describing content blocks 

{% note warning %}

The array must contain at least one element and no more than 20 elements.

{% endnote %}

||
|#

## `LogoDto` Object

Logo of the timeline entry.

### Parameters of the `LogoDto` Object

{% include [Footnote on parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **code^*^**
[`string`](../../../../data-types.md) | Logo code, for example `call`. A list of available codes can be obtained using the [crm.timeline.logo.list](../../../logmessage/logo/crm-timeline-logo-list.md) method ||
|| **action**
[`ActionDto`](./action.md) | Action to be taken when the logo is clicked ||
|#

## Example Object (without content blocks)

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

## Continue Your Exploration

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