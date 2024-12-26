# Dropdown Menu Item for the Top Button of the CRM_XXX_ROBOT_DESIGNER_TOOLBAR

> Scope: [`intranet`](../../scopes/permissions.md)

You can add your dropdown menu item to the top button of the Automation rule designer for CRM objects such as [leads](../../crm/leads/index.md) and [deals](../../crm/deals/index.md).

The specific placement code for the widget is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

## Where the Widget is Embedded

#|
|| **Widget Code** | **Location** ||
|| `CRM_LEAD_ROBOT_DESIGNER_TOOLBAR` | Dropdown menu item for the top button of the Automation rule designer in [lead](../../crm/leads/index.md) ||
|| `CRM_DEAL_ROBOT_DESIGNER_TOOLBAR` | Dropdown menu item for the top button of the Automation rule designer in [deal](../../crm/deals/index.md) ||
|#

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

{% list tabs %}

- CRM_LEAD_ROBOT_DESIGNER_TOOLBAR

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 2ce63de88c4a9f5843e148d6f7b7a6ed
        [AUTH_ID] => d54fba6600631fcd00005a4b00000001f0f1073f6f5fc879c485f124cc572c68a6ee17
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => c5cee16600631fcd00005a4b00000001f0f107833fc0c197d37b9b13905b691787bbdb
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_LEAD_ROBOT_DESIGNER_TOOLBAR
    )

    ```

- CRM_DEAL_ROBOT_DESIGNER_TOOLBAR

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => aa01af1bd7f74d944ab61bdc8ed4f011
        [AUTH_ID] => ec4fba6600631fcd00005a4b00000001f0f107219e88649824f5ded51f56111616561c
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => dccee16600631fcd00005a4b00000001f0f107021a4718dc94fa53f048dac305baff48
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_DEAL_ROBOT_DESIGNER_TOOLBAR
    )

    ```

{% endlist %}

{% include [Footnote on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

In the current widget, the `PLACEMENT_OPTIONS` parameter is not passed.

## Continue Your Exploration

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)