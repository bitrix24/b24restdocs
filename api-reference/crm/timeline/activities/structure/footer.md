# Bottom Part of the Record

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet writing standards
- are the types other than standard specified correctly?

{% endnote %}

{% endif %}

## FooterDto

The bottom part of the timeline entry with the action block.

#|
|| **Field** | **Description** | **Additional** ||
|| **buttons**
[`FooterButtonDto`](#footerbuttondto) | An array of objects describing action buttons. | No more than two buttons are allowed. ||
|| **menu**
[`FooterMenuDto`](#footermenudto) | Bottom menu | ||
|#

## FooterButtonDto

Button in the bottom part of the timeline entry.

#|
|| **Field** | **Description** | **Additional** ||
|| **title^*^**
[`textWithTranslation`](./field-types.md) | Button text | Required ||
|| **type^*^**
[`string`](../../../../data-types.md) | Button type. Defines its appearance. | Required ||
|| **action^*^**
[`ActionDto`](./action.md) | Action triggered by pressing the button. | Required ||
|| **scope**
[`string`](../../../../data-types.md) | Where to display | ||
|| **hideIfReadonly**
[`boolean`](../../../../data-types.md) | Hide if the user does not have edit access. | Default is `false`. ||
|#

{% include [Footnote on parameters](../../../../../_includes/required.md) %}

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

Dropdown menu in the bottom part of the timeline entry.

#|
|| **Field** | **Description** | **Additional** ||
|| **showPinItem**
[`boolean`](../../../../data-types.md) | Whether to show the "Pin" menu item. The menu item will not be shown if added to an incomplete deal. | Default is `true`. ||
|| **showPostponeItem**
[`boolean`](../../../../data-types.md) | Whether to show the "Postpone" menu item. The menu item will not be shown if added to an incoming deal, a deal without a deadline, or a completed deal. | Default is `true`. ||
|| **showDeleteItem**
[`boolean`](../../../../data-types.md) | Whether to show the "Delete" menu item. | Default is `true`. ||
|| **items**
[`MenuItemDto`](./menu-item.md) | Associative array of objects describing dropdown menu items | ||
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