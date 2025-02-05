# Bottom Dropdown Menu

Items of the bottom dropdown menu of the timeline entry `MenuItemDto`.

## Parameters of the `MenuItemDto` Object

{% include [Footnote on parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **title^*^**
[`textWithTranslation`](./field-types.md#textwithtranslation) | Button text ||
|| **action^*^**
[`ActionDto`](./action.md) | Action performed when the button is clicked ||
|| **scope**
[`string`](../../../../data-types.md) | [Scope](./field-types.md#scope), for example `web` ||
|| **hideIfReadonly**
[`boolean`](../../../../data-types.md) | Flag. Hides the tag if the user does not have edit access (default is `false`) ||
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