# Menu Item in CRM Sales Funnels Toolbar

> Scope: [`intranet`](../../scopes/permissions.md)

You can add your item to the sales funnels toolbar.

![Widget as an item in the sales funnels toolbar](./_images/CRM_FUNNELS_TOOLBAR.png "Widget as an item in the sales funnels toolbar")

The specific widget placement code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The widget will not be displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

#|
|| **Widget Code** | **Location** ||
|| `CRM_FUNNELS_TOOLBAR` | Item in the sales funnels toolbar ||
|#

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

```php

Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => 4c8dff86744e675bbb861d69003fbbe9
    [AUTH_ID] => 574fba6600631fcd00005a4b00000001f0f107066649fd06e706777e462385fca29ac6
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 47cee16600631fcd00005a4b00000001f0f107f2599a4c4602cfa840b6f9765e5f30c7
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => CRM_FUNNELS_TOOLBAR
)

```

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

In the current widget, the `PLACEMENT_OPTIONS` parameter is not passed.

## Continue Your Exploration

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../../settings/interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)