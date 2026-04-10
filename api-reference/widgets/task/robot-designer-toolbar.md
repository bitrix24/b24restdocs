# Main Dropdown Menu Item Next to Automation Rules Settings TASK_ROBOT_DESIGNER_TOOLBAR

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`intranet`](../../scopes/permissions.md)

You can add your own main dropdown menu item next to the settings for Automation Rules in tasks.

The code for the specific widget placement is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The widget will not be displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the widget is embedded

#| 
|| **Widget code** | **Location** ||
|| `TASK_ROBOT_DESIGNER_TOOLBAR` | Main dropdown menu item next to Automation Rules settings ||
|#

## What the handler receives

Data is transmitted as a POST request {.b24-info}

```php
Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => 4617fa96af5d1f523fc2e2b72bd54f11
    [AUTH_ID] => 5253ba6600705a0700005a4b00000001f0f1076fef51e6d3d3c1616a9fd92a714ca452
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 42d2e16600705a0700005a4b00000001f0f107cf69d8060249da353587f8ec862be702
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => TASK_USER_LIST_TOOLBAR
    [PLACEMENT_OPTIONS] => {"USER_ID":"1"}
)
```

{% include [Note on required parameters](../../../_includes/required.md) %}

{% include notitle [description of standard data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is a JSON string containing an array of one or more keys.

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Parameter** | **Description** ||
|| **USER_ID*** 
[`string`](../../data-types.md) | The identifier of the user whose Automation Rules settings opened the widget.

It can be used to retrieve additional information using the [user.get](../../user/user-get.md) method.

|| 
|#

## Continue your exploration

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../../../settings/interactivity/index.md)
- [{#T}](../bx24-widget-methods.md)