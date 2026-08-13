# Widgets in User Profile: Overview of Placements

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The placements of this section add an application item where the user works with profiles: to the user's own menu and to the employee card. Both placements require the `user` scope.

To register a widget, use the [placement.bind](../placement-bind.md) method and pass the required code in the `PLACEMENT` parameter.

> Quick navigation: [all placements](#all-placements)

## How to Choose a Placement

The placements differ in whose identifier the handler receives.

#|
|| **Placement** | **Where the User Sees It** | **When to Use** ||
|| [USER_PROFILE_MENU](./profile-menu.md) | In the menu under the avatar in the upper right corner, via the *Extensions* button | When the action relates to the user themselves: personal application settings, a jump to your own section. The handler receives the identifier of the person who opened the menu ||
|| [USER_PROFILE_TOOLBAR](./profile-toolbar.md) | In the menu of the button in the upper right corner of the employee card | When the action relates to a specific employee: data from an external system, a request about a person. The handler receives the identifier of the owner of the open card ||
|#

## How to Get Started

1. Choose the placement by whose identifier the handler needs
2. Register the handler with the [placement.bind](../placement-bind.md) method, pass the placement code in `PLACEMENT` and the item name in `TITLE`
3. Complete the application installation — until then the item does not appear in the interface
4. Open the user menu or an employee card and select your item
5. Parse `PLACEMENT_OPTIONS` in the handler and retrieve employee data with the [user.get](../../user/user-get.md) method

## What the Handler Receives

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

```php
Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => bbdb976c9f5d067b1d48d102ab17b995
    [AUTH_ID] => ae70bb6600705a0700005a4b00000001f0f107ab19f75f907d2320df1129aa61f63efc
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 9eefe26600705a0700005a4b00000001f0f1078586205803785eca5262f6ff48e025ee
    [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
    [APPLICATION_TOKEN] => 5b2f8c1d7e3a9046b8c5d2f1a7e3b904
    [APPLICATION_SCOPE] => user,placement
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => USER_PROFILE_MENU
    [PLACEMENT_OPTIONS] => {"USER_ID":"1","URI":"\/company\/"}
)
```

{% include [Note on required parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is passed as a JSON string with the call context. The set of keys is the same for both placements, but the value of `USER_ID` differs.

#|
|| **Placement** | **Keys** | **What Is in USER_ID** ||
|| [USER_PROFILE_MENU](./profile-menu.md) | `USER_ID`, `URI` | The current user — the one who opened the menu ||
|| [USER_PROFILE_TOOLBAR](./profile-toolbar.md) | `USER_ID`, `URI` | The owner of the open employee card ||
|#

The `URI` key is universal: it carries the address of the page the widget is opened from.

Neither placement supports the `OPTIONS` parameter of the [placement.bind](../placement-bind.md) method: the values passed are not retained, and [placement.get](../placement-get.md) returns an empty array.

## Relationship With Other Objects

**User.** Using `USER_ID` from the call context, the application retrieves employee data with the [user.get](../../user/user-get.md) method. The list of employees and the company structure are returned by the methods of the [{#T}](../../user/index.md) and [{#T}](../../departments/index.md) sections.

## Common Mistakes

#|
|| **Mistake** | **Solution** ||
|| The item did not appear after registration | Complete the application installation and reload the page: the menu and the card build the item list on load ||
|| The handler receives the wrong employee | Check the placement code: `USER_PROFILE_MENU` returns the current user, while `USER_PROFILE_TOOLBAR` returns the owner of the open card ||
|| The `OPTIONS` passed during registration does not reach the handler | The placements of this section do not support `OPTIONS`. Pass your values through the handler address ||
|#

## Overview of Placements {#all-placements}

> Scope: [`placement, user`](../../scopes/permissions.md)

#|
|| **Placement** | **When to Use** ||
|| [USER_PROFILE_MENU](./profile-menu.md) | An item in the user menu, available from any page of Bitrix24 ||
|| [USER_PROFILE_TOOLBAR](./profile-toolbar.md) | An item in the employee card menu ||
|#

## Continue Learning

- [{#T}](../placement-bind.md)
- [{#T}](../placement-list.md)
- [{#T}](../placement-unbind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../bx24-widget-methods.md)
- [{#T}](../../user/index.md)
- [{#T}](../../../settings/interactivity/index.md)
