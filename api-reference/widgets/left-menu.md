# Widgets in the LEFT_MENU Main Menu

> Scope: [`basic`](../scopes/permissions.md)

You can add your item to the main menu of the account.

![Widget as an item in the main menu of the account](./_images/LEFT_MENU.png "Widget as an item in the main menu of the account")

The specific placement code for the widget is specified in the `PLACEMENT` parameter of the [placement.bind](./placement-bind.md) method.

{% note info "" %}

The widget will not be displayed in the interface until the application installation is complete. [Check the application installation](../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the widget is embedded

#| 
|| **Placement Code** | **Location** ||
|| `LEFT_MENU` | Item in the main menu of the account ||
|#

## What the handler receives

Data is transmitted as a POST request {.b24-info}

```php

Array
(
    [handler] => 1
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => fea0d7bc24669fcb8807e88ee394c7ca
    [AUTH_ID] => 63d39f6600631fcd00005a4b00000001f0f1071905299b72b307a6c223d43877697546
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 5352c76600631fcd00005a4b00000001f0f107d262f083bb53a16948269371e327d1d9
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => LEFT_MENU
)

```

{% include notitle [Footnote on required parameters](../../_includes/required.md) %}

{% include notitle [description of standard data](_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

In the current widget, the `PLACEMENT_OPTIONS` parameter is not passed.

{% note tip "Typical use-cases and scenarios" %}

- [Application with its own page in the left menu](https://dev.quickbooks.com/learning/course/index.php?COURSE_ID=266&LESSON_ID=25538&LESSON_PATH=25398.25506.25530.25538)

{% endnote %}

## Continue your study

- [{#T}](./placement-bind.md)
- [{#T}](./ui-interaction/index.md)
- [{#T}](./ui-interaction/crm-card.md)
- [{#T}](../../settings/interactivity/index.md)
- [{#T}](./open-application.md)
- [{#T}](./open-path.md)