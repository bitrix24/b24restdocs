# Main Dropdown Menu Item Near Automation Rule Settings TASK_ROBOT_DESIGNER_TOOLBAR

> Scope: [`intranet`](../../scopes/permissions.md)

You can add your own main dropdown menu item near the Automation Rule settings in workgroups.

The code for the specific widget placement is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

## Where the Widget is Embedded

#|
|| **Widget Code** | **Location** ||
|| `TASK_ROBOT_DESIGNER_TOOLBAR` | Main dropdown menu item near Automation Rule settings ||
|#

## What the Handler Receives

Data is sent as a POST request {.b24-info}

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
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => TASK_ROBOT_DESIGNER_TOOLBAR
    [PLACEMENT_OPTIONS] => {"GROUP_ID":"10"}
)

```

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is a JSON string containing an array of one or more keys.

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Parameter** | **Description** ||
|| **GROUP_ID***
[`string`](../../data-types.md) | Identifier of the workgroup/project where the widget was opened.

Can be used to obtain additional information using the [sonet.group.get](../../sonet-group/sonet-group-get.md) method.

||
|#

## Continue Exploring

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)