# Tab in the Task Card TASK_VIEW_TAB

> Scope: [`task`](../../scopes/permissions.md)

You can add your own tab in the task card.

The specific widget placement code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

## Where the Widget is Embedded

#|
|| **Widget Code** | **Location** ||
|| `TASK_VIEW_TAB` | Tab in the task card ||
|#

## What the Handler Receives

Data is sent as a POST request {.b24-info}

```php
Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => 0063a02ba25315469678f946ece50010
    [AUTH_ID] => 9c52ba6600705a0700005a4b00000001f0f107e81691773d119eb941ad045e362fcb2a
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 8cd1e16600705a0700005a4b00000001f0f1070aef2cbe270a6f27bcaf791e454e1047
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => TASK_VIEW_TAB
    [PLACEMENT_OPTIONS] => {"taskId":"286"}
)
```

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is a JSON string containing an array of one or more keys.

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Parameter** | **Description** ||
|| **taskId***
[`string`](../../data-types.md) | The identifier of the task for which the widget was opened.

It can be used to retrieve additional information using the [tasks.task.get](../../tasks/tasks-task-get.md) method.

||
|#

## Continue Your Exploration

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)