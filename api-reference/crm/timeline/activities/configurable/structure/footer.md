# Bottom Part of the Record

The bottom part of the [timeline record](../index.md) with the `FooterDto` action block.

## Parameters of the `FooterDto` Object

{% include [Footnote on parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **buttons**
[`FooterButtonDto`](#footerbuttondto) | An array of objects describing action buttons. No more than two buttons are allowed ||
|| **menu**
[`FooterMenuDto`](#footermenudto) | Bottom menu ||
|#

## FooterButtonDto

A button in the bottom part of the timeline record.

### Parameters of the `FooterButtonDto` Object

{% include [Footnote on parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **title^*^**
[`textWithTranslation`](./field-types.md#textwithtranslation) | Button text ||
|| **type^*^**
[`string`](../../../../data-types.md) | Button type. Determines its appearance, e.g., `primary` ||
|| **action^*^**
[`ActionDto`](./action.md) | Action performed when the button is pressed ||
|| **scope**
[`string`](../../../../data-types.md) | [Scope](./field-types.md#scope), e.g., `web` ||
|| **hideIfReadonly**
[`boolean`](../../../../data-types.md) | Flag. Hides the tag if the user does not have edit access (default is `false`) ||
|#

Possible values for the **type** field:

- **primary** - Blue button background
- **secondary** - White button background

### Example

```json
{
    "title": "Open deal",
    "type": "primary",
    "action": {
        "type": "redirect",
        "uri": "/crm/deal/details/123/"
    },
    "scope": "web",
    "hideIfReadonly": true
}
```

## FooterMenuDto

Dropdown menu in the bottom part of the timeline record.

### Parameters of the `FooterMenuDto` Object

#|
|| **Field** | **Description** ||
|| **showPinItem**
[`boolean`](../../../../data-types.md) | Show the "Pin" menu item. The menu item will not be shown if added to an incomplete deal. Default is `true` ||
|| **showPostponeItem**
[`boolean`](../../../../data-types.md) | Show the "Postpone" menu item. The menu item will not be shown if added to an incoming deal, a deal without a deadline, or a completed deal. Default is `true`. ||
|| **showDeleteItem**
[`boolean`](../../../../data-types.md) | Show the "Delete" menu item. Default is `true` ||
|| **items**
[`MenuItemDto`](./menu-item.md) | Associative array of objects describing dropdown menu items ||
|#

### Example

```json
{
    "showPostponeItem": "false",
    "showDeleteItem": "false",
    "items": {
        "confirm": {
            "title": "Confirm request",
            "action": {
                "type": "restEvent",
                "id": "confirm",
                "animationType": "loader"
            }
        },
        "decline": {
            "title": "Decline request",
            "action": {
                "type": "restEvent",
                "id": "decline",
                "animationType": "loader"
            }
        }
    }
}
```

## Continue Learning

- [{#T}](./layout.md)
- [{#T}](./icon.md)
- [{#T}](./body.md)
- [{#T}](./content-block.md)
- [{#T}](./header.md)
- [{#T}](./menu-item.md)
- [{#T}](./action.md)
- [{#T}](./field-types.md)
- [{#T}](./rest-app-layout-dto.md)
- [{#T}](./examples.md)