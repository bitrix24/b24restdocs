# Tab in the task card TASK_VIEW_TAB

> Scope: [`task`](../../scopes/permissions.md)

You can add your item in the right panel of the old task detail form.

Starting from version `tasks 25.700.0`, a [new task card](../../tasks/tasks-new.md) has been released. The location of the `TASK_VIEW_TAB` item is no longer present in the new card. All widgets within the card are displayed in a single Applications block.

Previously registered `TASK_VIEW_TAB` items continue to function and are displayed in the Applications block.

![Embedded applications](_images/widget.png)

The specific placement code for the widget is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The widget will not be displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the widget is embedded

#|
|| **Widget code** | **Location** ||
|| `TASK_VIEW_TAB` | Tab in the task card ||
|#

## What the handler receives

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

{% include [Note on required parameters](../../../_includes/required.md) %}

{% include notitle [description of standard data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is a JSON string containing an array of one or more keys.

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Parameter** | **Description** ||
|| **taskId***
[`string`](../../data-types.md) | The identifier of the task for which the widget was opened.

It can be used to retrieve additional information using the [tasks.task.get](../../tasks/tasks-task-get.md) method.

||
|#

## Continue exploring

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../../settings/interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)