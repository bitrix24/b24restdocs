# Dropdown Menu Item of the Top Button in CRM Analytics CRM_ANALYTICS_TOOLBAR

> Scope: [`intranet`](../../scopes/permissions.md)

You can add your own dropdown menu item to the top button in CRM Analytics.

![Widget as an item in the CRM Analytics toolbar](./_images/CRM_ANALYTICS_TOOLBAR.png "Widget as an item in the CRM Analytics toolbar")

The specific placement code for the widget is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

## Where the Widget is Embedded

#|
|| **Widget Code** | **Location** ||
|| `CRM_ANALYTICS_TOOLBAR` | Dropdown menu item of the top button in CRM Analytics ||
|#

## What the Handler Receives

Data is sent as a POST request {.b24-info}

```php

Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => d3d342f98a82fc313fbfb1e9fe172163
    [AUTH_ID] => 1f4fba6600631fcd00005a4b00000001f0f10702ea003068e6f5e38d8290cfcdfe300c
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 0fcee16600631fcd00005a4b00000001f0f1070dfad25ad4247da6dcb566292a0da3f3
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => CRM_ANALYTICS_TOOLBAR
)

```

{% include [Note on required parameters](../../../_includes/required.md) %}

{% include notitle [description of standard data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

In the current widget, the `PLACEMENT_OPTIONS` parameter is not passed.

## Continue Your Exploration

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../../settings/interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)