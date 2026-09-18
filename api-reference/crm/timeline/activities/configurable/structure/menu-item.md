# Bottom Dropdown Menu

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

`MenuItemDto` represents an item in the dropdown menu at the bottom of the [timeline entry](../index.md). Click the three-dot button to open the menu. The application adds its own actions to it — for example, to confirm or decline a request.

Items are passed in the `items` field of the [FooterMenuDto](./footer.md#footermenudto) object: the key is an item identifier made of Latin letters, digits, hyphens, and underscores, and the value is a `MenuItemDto` object. The application can add no more than 10 items. In the activity structure, the path to an item is `layout.footer.menu.items.<key>`, and the structure is passed in the `layout` parameter of the methods [crm.activity.configurable.add](../crm-activity-configurable-add.md) and [crm.activity.configurable.update](../crm-activity-configurable-update.md). The call conditions and validation error codes are on the method pages.

After the application items, Bitrix24 adds the system items "Pin", "Postpone", and "Delete" to the same menu. The `showPinItem`, `showPostponeItem`, and `showDeleteItem` flags of the `FooterMenuDto` object allow them to be shown, but whether Bitrix24 actually shows an item also depends on the activity state — the conditions are described on the [FooterMenuDto](./footer.md#footermenudto) page. Bitrix24 always adds the "About the application" item, and it cannot be disabled.

## Parameters of the `MenuItemDto` Object

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **title^*^**
[`textWithTranslation`](./field-types.md#textwithtranslation) | Menu item text ||
|| **action^*^**
[`ActionDto`](./action.md) | Action to be performed when the item is clicked ||
|| **subtitle**
[`textWithTranslation`](./field-types.md#textwithtranslation) | Second line of the item, below the text ||
|| **scope**
[`string`](../../../../../data-types.md) | [Scope](./field-types.md#scope), for example `web` ||
|| **hideIfReadonly**
[`boolean`](../../../../../data-types.md) | If `true`, the item is not shown to a user who does not have permission to edit the CRM item. Default is `false` ||
|| **design**
[`string`](../../../../../data-types.md) | Item style: `default`, `accent-1`, `accent-2`, `alert`, `copilot`, or `disabled`. Bitrix24 rejects any other value with the `ENUM_FIELD` error. An item with `disabled` looks unavailable, but the click action is still performed — for example, to explain why the item is disabled ||
|| **isSelected**
[`boolean`](../../../../../data-types.md) | Toggle item: with `true`, a check mark is shown; with `false`, an empty space for it. Without this field, the item is a regular one ||
|| **isLocked**
[`boolean`](../../../../../data-types.md) | If `true`, a lock icon is shown next to the item ||
|| **badgeText**
[`BadgeTextDto`](#badge-text-dto) | Badge next to the item text, for example "New" ||
|| **sectionCode**
[`string`](../../../../../data-types.md) | Code of the [menu section](#sections) the item belongs to. If sections are declared, the field is required for every item and must match the code of one of them ||
|#

## `BadgeTextDto` Object {#badge-text-dto}

A badge displayed next to a menu item.

### Parameters of the `BadgeTextDto` Object

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **title^*^**
[`textWithTranslation`](./field-types.md#textwithtranslation) | Badge text ||
|| **color**
[`string`](../../../../../data-types.md) | Badge color in CSS format, for example `#2fc6f6` ||
|#

## Menu Sections {#sections}

Items can be grouped into sections with headings. Sections are declared in the `sections` field of the `FooterMenuDto` object as an array of `MenuSectionDto` objects, no more than 10 sections. When sections are declared, every item in `items` must have a `sectionCode` with the code of one of them; otherwise, Bitrix24 rejects the structure. Bitrix24 places the system items in its own `system` section and the "About the application" item in the `about` section; the codes `system`, `about`, `base`, and `extensions` cannot be used for your own sections.

### Parameters of the `MenuSectionDto` Object

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **code^*^**
[`string`](../../../../../data-types.md) | Section code. It is specified in the `sectionCode` field of the items ||
|| **title**
[`textWithTranslation`](./field-types.md#textwithtranslation) | Section heading in the menu ||
|| **design**
[`string`](../../../../../data-types.md) | Section style: `default` or `accent` ||
|#

## Example Object

A menu item with a subtitle, accent style, and a badge. The item is hidden from users without permission to edit the item.

```json
{
    "title": "Confirm Request",
    "subtitle": "Send the contract to the client",
    "action": {
        "type": "restEvent",
        "id": "confirmRequest",
        "animationType": "loader"
    },
    "design": "accent-1",
    "badgeText": {
        "title": "New",
        "color": "#2fc6f6"
    },
    "hideIfReadonly": true
}
```

## Example Menu with Sections

The value of the `menu` field of the `FooterDto` object contains two application items in the "Request" section. The "Postpone" and "Delete" system items are disabled.

```json
{
    "showPostponeItem": false,
    "showDeleteItem": false,
    "sections": [
        {
            "code": "request",
            "title": "Request"
        }
    ],
    "items": {
        "confirm": {
            "title": "Confirm",
            "sectionCode": "request",
            "action": {
                "type": "restEvent",
                "id": "confirmRequest",
                "animationType": "loader"
            }
        },
        "decline": {
            "title": "Decline",
            "sectionCode": "request",
            "design": "alert",
            "action": {
                "type": "restEvent",
                "id": "declineRequest",
                "animationType": "loader"
            }
        }
    }
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