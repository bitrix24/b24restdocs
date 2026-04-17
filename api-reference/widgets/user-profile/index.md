# Widgets in User Profile: Overview of Embedding Points

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This section describes the embedding points for widgets in the Bitrix24 user profile interface. Through these points, developers can add their item to the context menu of the profile or to the context menu of the upper profile button.

To register a widget, use the [placement.bind](../placement-bind.md) method and pass the required code in the `PLACEMENT` parameter.

> Quick navigation: [all widgets](#all-widgets)

## How to Choose an Embedding Point

Both embedding points relate to the user profile but are displayed in different menus of the interface.

#|
|| **Embedding Point** | **Where the User Sees It** | **When to Use** ||
|| [USER_PROFILE_MENU](./profile-menu.md) | In the user menu under the *Extensions* button in the upper right corner | When the action should be accessible from the general user menu without navigating to the profile slider ||
|| [USER_PROFILE_TOOLBAR](./profile-toolbar.md) | In the context menu of the *Extensions* button in the upper right corner of the profile slider | When the action should be available within the open user profile and relate to working with the profile card ||
|#

## How to Get Started

1. Choose the embedding point for your scenario in the user profile.
2. Register the handler via [placement.bind](../placement-bind.md) and pass the required `PLACEMENT`.
3. Parse `PLACEMENT_OPTIONS` in the handler to obtain the user ID.
4. If necessary, use `USER_ID` to call methods from the [user](../../user/index.md) section and retrieve profile data.

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

{% list tabs %}

- USER_PROFILE_MENU

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
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => USER_PROFILE_MENU
        [PLACEMENT_OPTIONS] => {"USER_ID":"1"}
    )
    ```

- USER_PROFILE_TOOLBAR

    ```php
    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 8cd7740e289bf14997dd7e5e20cf6d13
        [AUTH_ID] => dc70bb6600705a0700005a4b00000001f0f1079c18b7c3d0497a2cf769e3c4d1150a9b
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => ccefe26600705a0700005a4b00000001f0f107961459d1f9ac07ba82616c72079ede7b
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => USER_PROFILE_TOOLBAR
        [PLACEMENT_OPTIONS] => {"USER_ID":"1"}
    )
    ```

{% endlist %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is passed as a JSON string with the context of the call.

#|
|| **Widget** | **Keys** | **Description** ||
|| [USER_PROFILE_MENU](./profile-menu.md) | `USER_ID` | The identifier of the user whose profile the widget is opened in ||
|| [USER_PROFILE_TOOLBAR](./profile-toolbar.md) | `USER_ID` | The identifier of the user whose profile the widget is opened in ||
|#

## Connection with Other Objects

**User.** The `USER_ID` parameter in `PLACEMENT_OPTIONS` indicates for which user the handler was called. You can obtain user information by ID using the [user.get](../../user/user-get.md) method.

## Typical Errors

#|
|| **Error** | **How to Resolve** ||
|| The widget does not display after registration | Complete the application installation and reopen the user profile ||
|| The expected user does not come to the handler | Ensure that the handler is registered for the correct `PLACEMENT` and parses `USER_ID` from `PLACEMENT_OPTIONS` ||
|#

## Overview of Widgets {#all-widgets}

> Scope: [`user`](../../scopes/permissions.md)

#|
|| **Widget** | **When to Use** ||
|| [USER_PROFILE_MENU](./profile-menu.md) | Context menu item in the user profile ||
|| [USER_PROFILE_TOOLBAR](./profile-toolbar.md) | Context menu item for the upper profile button ||
|#