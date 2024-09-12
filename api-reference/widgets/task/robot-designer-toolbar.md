# Main Dropdown Menu Item Near Automation Rules Settings TASK_ROBOT_DESIGNER_TOOLBAR

> Scope: [`intranet`](../../scopes/permissions.md)

You can add your own main dropdown menu item near the settings of Automation rules in tasks.

The code for the specific widget placement is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

## Where the Widget is Embedded

#|
|| **Widget Code** | **Location** ||
|| `TASK_ROBOT_DESIGNER_TOOLBAR` | Main dropdown menu item near Automation rules settings ||
|#

## What the Handler Receives

Data is sent as a POST request {.b24-info}

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

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is a JSON string containing an array of one or more keys.

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Parameter** | **Description** ||
|| **USER_ID***
[`string`](../../data-types.md) | The identifier of the user whose Automation rules settings opened the widget.

Can be used to obtain additional information using the [user.get](../../user/user-get.md) method.

||
|#

## Continue Your Exploration

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)