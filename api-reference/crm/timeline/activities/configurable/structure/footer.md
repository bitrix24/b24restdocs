# Bottom Part of a Configurable Activity Record

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

An application displays its own actions below a configurable activity record in a CRM card timeline — for example, to confirm a request or open a related deal. These actions are defined by the `FooterDto` object. It describes the bottom part of the [timeline record](../index.md): buttons, the "Note" icon, and the menu that opens from the three-dot icon.

The object is passed in the `footer` field of the [configurable activity structure](./layout.md) when calling the [crm.activity.configurable.add](../crm-activity-configurable-add.md) and [crm.activity.configurable.update](../crm-activity-configurable-update.md) methods. The path in the request is `layout.footer`: buttons go in `layout.footer.buttons`, menu items in `layout.footer.menu.items`.

Menu items are described on the [dropdown menu](./menu-item.md) page. Ready-made field combinations are collected in the [activity configuration examples](./examples.md).

## Buttons or Menu

An application can show its actions in two ways:

- buttons — up to two, displayed right below the record and intended for primary actions
- menu items — up to ten, available from the three-dot icon and optionally grouped into sections with headings

Bitrix24 adds some elements itself: the "Note" icon and the "Pin", "Postpone", and "Delete" system menu items. Fields of the object can disable them. The "About the application" item is always present in the browser. If `footer` is not passed, the record shows only these system elements, without application buttons.

## Parameters of the `FooterDto` Object

#|
|| **Field** | **Description** ||
|| **buttons**
[`object`](../../../../../data-types.md) | Action buttons: the key is a button identifier that you choose, the value is a [FooterButtonDto](#footerbuttondto) object. The key can contain Latin letters, digits, hyphens, and underscores. Pass no more than two buttons ||
|| **showNote**
[`boolean`](../../../../../data-types.md) | If `false`, the record has no "Note" icon. The icon is shown by default ||
|| **menu**
[`FooterMenuDto`](#footermenudto) | The menu that opens from the three-dot icon ||
|#

If the structure violates these restrictions, the method returns a validation error. General codes are listed on the [crm.activity.configurable.add](../crm-activity-configurable-add.md#errors) and [crm.activity.configurable.update](../crm-activity-configurable-update.md#errors) pages, and codes for menu sections are in the [FooterMenuDto description](#footermenudto).

{% note warning "" %}

Pass `buttons` as an object with keys. The method accepts an array without keys and returns no error, but the buttons do not appear in the record.

{% endnote %}

## `FooterButtonDto` Object {#footerbuttondto}

A button in the bottom part of the timeline record.

### Parameters of the `FooterButtonDto` Object

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **title^*^**
[`textWithTranslation`](./field-types.md#textwithtranslation) | Button text ||
|| **type^*^**
[`string`](../../../../../data-types.md) | Button style: `primary` is the main button, `secondary` is an additional button ||
|| **action^*^**
[`ActionDto`](./action.md) | Action to be performed when the button is clicked ||
|| **scope**
[`string`](../../../../../data-types.md) | [Scope](./field-types.md#scope), e.g., `web` ||
|| **hideIfReadonly**
[`boolean`](../../../../../data-types.md) | If `true`, the button is not shown to a user who does not have permission to edit the CRM object. Default is `false` ||
|#

## `FooterMenuDto` Object {#footermenudto}

The menu that opens from the three-dot icon. The `showPinItem`, `showPostponeItem`, and `showDeleteItem` flags disable system items, and the `items` and `sections` fields define the application's items.

### Parameters of the `FooterMenuDto` Object

#|
|| **Field** | **Description** ||
|| **showPinItem**
[`boolean`](../../../../../data-types.md) | If `false`, the menu has no "Pin" item. If `true`, Bitrix24 shows the item only for a completed activity. Default is `true` ||
|| **showPostponeItem**
[`boolean`](../../../../../data-types.md) | If `false`, the menu has no "Postpone" item. If `true`, Bitrix24 shows the item only for an incomplete activity with a deadline. An incoming activity never has a deadline, so it has no such item either. Default is `true` ||
|| **showDeleteItem**
[`boolean`](../../../../../data-types.md) | If `false`, the menu has no "Delete" item. Default is `true` ||
|| **items**
[`object`](../../../../../data-types.md) | Application items: the key is the item identifier, the value is a [MenuItemDto](./menu-item.md) object. No more than ten items ||
|| **sections**
[`array`](../../../../../data-types.md) | Menu sections with headings — an array of `MenuSectionDto` objects, no more than ten. If sections are passed, every item must have a `sectionCode`. Section fields and rules are described in the [Menu Sections](./menu-item.md#sections) block ||
|#

If menu sections are set incorrectly, the method returns one of the following codes:

#|
|| **Code** | **Reason** ||
|| `MENU_RESERVED_SECTION_CODE` | The section code matches a reserved one: `system`, `about`, `base`, or `extensions` ||
|| `MENU_DUPLICATE_SECTION_CODE` | Two sections have the same code ||
|| `MENU_MISSING_SECTION_CODE` | Sections are passed, but an item has no `sectionCode` ||
|| `MENU_UNKNOWN_SECTION_CODE` | The item's `sectionCode` does not match the code of any section ||
|#

## Object Example

The value of the `footer` field with two buttons. Only users with permission to edit the deal see the "Confirm request" button. The "Open deal" button is shown only in the browser. There is no "Note" icon. The menu contains the "Decline request" application item, and the "Postpone" and "Delete" system items are disabled.

```json
{
    "buttons": {
        "confirm": {
            "title": "Confirm request",
            "type": "primary",
            "action": {
                "type": "restEvent",
                "id": "confirm",
                "animationType": "loader"
            },
            "hideIfReadonly": true
        },
        "open": {
            "title": "Open deal",
            "type": "secondary",
            "action": {
                "type": "redirect",
                "uri": "/crm/deal/details/123/"
            },
            "scope": "web"
        }
    },
    "showNote": false,
    "menu": {
        "showPostponeItem": false,
        "showDeleteItem": false,
        "items": {
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
}
```

## Permissions and Limitations

> Scope: [`crm`](../../../../../scopes/permissions.md)
>
> Who can execute the method: a user with edit access to the CRM object the activity is linked to. Without such access the method returns the `ACCESS_DENIED` error

Limitations:

- the [crm.activity.configurable.add](../crm-activity-configurable-add.md) and [crm.activity.configurable.update](../crm-activity-configurable-update.md) methods work only within the context of an [application](../../../../../../settings/app-installation/index.md): when they are called via a webhook, Bitrix24 returns the `ERROR_WRONG_CONTEXT` error
- only the application that created the activity can update it with the [crm.activity.configurable.update](../crm-activity-configurable-update.md) method

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
