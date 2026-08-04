# Workgroup Extensions Menu Item SONET_GROUP_TOOLBAR

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`sonet_group`](../../scopes/permissions.md)

The widget adds its own item to the extensions menu of a workgroup or project.

The placement code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The widget will not be displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

#|
|| **Widget Code** | **Location** ||
|| `SONET_GROUP_TOOLBAR` | Extensions menu item of a workgroup or project ||
|#

### Where to Find It in the Interface

Open the workgroup, click *•••* to the right of the group name and select *Extensions*. The application item is displayed in this submenu next to the knowledge base items and Bitrix24 Market.

{% note warning "" %}

The group menu with the *Extensions* item exists only in the classic interface. In the new Projects AI view, the placement is registered by the `placement.bind` method, but it has no rendering location — the item will not appear. To add your own item to the workgroup menu in both cases, use [{#T}](./index.md)

{% endnote %}

![Extensions menu item of a workgroup or project](./_images/SONET_GROUP_TOOLBAR.png "Extensions menu item of a workgroup or project")

## What the Handler Receives

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

```php
Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => 25e596577c2a1ddf98c7863421330527
    [AUTH_ID] => 5d56ba6600705a0700005a4b00000001f0f107d21c0babb82529a32836e165141a2010
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 4dd5e16600705a0700005a4b00000001f0f107a934a327935855b75f8c3686204e3bd5
    [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
    [APPLICATION_TOKEN] => 5b2f8c1d7e3a9046b8c5d2f1a7e3b904
    [APPLICATION_SCOPE] => sonet_group,task,placement
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => SONET_GROUP_TOOLBAR
    [PLACEMENT_OPTIONS] => {"URI":"\/workgroups\/group\/10\/tasks\/"}
)
```

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is passed as a JSON string with the context of the call.

`SONET_GROUP_TOOLBAR` has no keys of its own — the context contains only the universal `URI` key. The workgroup identifier does not arrive as a separate parameter, so take it from the path in `URI`. For the value `/workgroups/group/10/tasks/`, the workgroup identifier is `10`. Use it to retrieve the workgroup data with the [sonet_group.get](../../sonet-group/sonet-group-get.md) method.

If your handler needs the workgroup identifier explicitly, use [{#T}](./index.md) or [{#T}](./robot-designer-toolbar.md) — these placements pass it in the `GROUP_ID` key.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./robot-designer-toolbar.md)
- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../bx24-widget-methods.md)
- [{#T}](../../../settings/interactivity/index.md)
