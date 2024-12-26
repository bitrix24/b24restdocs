# Context Menu Item for CRM Activity in CRM Entity Card_XXX_ACTIVITY_TIMELINE_MENU

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)

You can add your own context menu item for CRM entities such as [leads](../../crm/leads/index.md) and [deals](../../crm/deals/index.md).

The specific placement code for the widget is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

## Where the Widget is Embedded

#|
|| **Widget Code** | **Location** ||
|| `CRM_LEAD_ACTIVITY_TIMELINE_MENU` | Context menu item for a [lead](../../crm/leads/index.md) ||
|| `CRM_DEAL_ACTIVITY_TIMELINE_MENU` | Context menu item for a [deal](../../crm/deals/index.md) ||
|| `CRM_QUOTE_ACTIVITY_TIMELINE_MENU` | Context menu item for an [estimate](../../crm/quote/index.md) ||
|| `CRM_DYNAMIC_XXX_ACTIVITY_TIMELINE_MENU` | Context menu item for a [custom object type](../../crm/universal/index.md) ||
|#

## What the Handler Receives

Data is sent as a POST request {.b24-info}

{% list tabs %}

- CRM_LEAD_ACTIVITY_TIMELINE_MENU

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => e6a18a6abe41ba3bd944897d8dc5186d
        [AUTH_ID] => 45d4a06600631fcd00005a4b00000001f0f10767246d86a580fae119d2a2601665eb33
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 3553c86600631fcd00005a4b00000001f0f1074bdbcddd7232d3413e2b9ff1ee91dc96
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_LEAD_ACTIVITY_TIMELINE_MENU
        [PLACEMENT_OPTIONS] => {"ENTITY_ID":"6591","TYPE_ID":"1","TYPE_CATEGORY_ID":"6","ASSOCIATED_ENTITY_ID":"1523","ASSOCIATED_ENTITY_TYPE_ID":"6","TIMELINE_ITEM_ID":"29937"}
    )

    ```

- CRM_DEAL_ACTIVITY_TIMELINE_MENU

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 71cc8c707a630542d5e2cf7435bf88c4
        [AUTH_ID] => 87d4a06600631fcd00005a4b00000001f0f10758d4aabbf27967a9af747de646c5447c
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 7753c86600631fcd00005a4b00000001f0f1078ec2dd6e94d5dc8f1aebf6524a86ee78
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_DEAL_ACTIVITY_TIMELINE_MENU
        [PLACEMENT_OPTIONS] => {"ENTITY_ID":"3463","ASSOCIATED_ENTITY_ID":"1517","ASSOCIATED_ENTITY_TYPE_ID":"6"}
    )

    ```

- CRM_QUOTE_ACTIVITY_TIMELINE_MENU

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => a63d2f88ffd62d6a300aaea7bf5ebf32
        [AUTH_ID] => 757ba26600631fcd00005a4b00000001f0f107b07473a33f9378bf912d602ecb056119
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 65fac96600631fcd00005a4b00000001f0f10726b77ceb5a0aaa50e143b1086fa03324
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_QUOTE_ACTIVITY_TIMELINE_MENU
        [PLACEMENT_OPTIONS] => {"ENTITY_ID":"5","ASSOCIATED_ENTITY_ID":"1529","ASSOCIATED_ENTITY_TYPE_ID":"6"}
    )
    
    ```

- CRM_DYNAMIC_XXX_ACTIVITY_TIMELINE_MENU

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => d659900f7e966c5d135d349b7b7f3c0d
        [AUTH_ID] => 4a7ba26600631fcd00005a4b00000001f0f1071e1f009645e93bf550bc02ac4f8fdcf6
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 3afac96600631fcd00005a4b00000001f0f107adfab4bb11ae71eaf061319f2b4b2f87
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_DYNAMIC_183_ACTIVITY_TIMELINE_MENU
        [PLACEMENT_OPTIONS] => {"ENTITY_ID":"3","ASSOCIATED_ENTITY_ID":"1527","ASSOCIATED_ENTITY_TYPE_ID":"6"}
    )
    
    ```

{% endlist %}

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

#### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is a JSON string containing an array of one or more keys.

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Parameter** | **Description** ||
|| **ENTITY_ID***
[`string`](../../data-types.md) | Identifier of the CRM entity for which the widget was opened.

Can be used to retrieve additional information using the corresponding methods:

- any entity type [crm.item.get](../../crm/universal/crm-item-get.md) with entityTypeId = '1' for leads, '2' for deals, and [etc.](../../crm/data-types.md#object_type)
- lead [crm.lead.get](../../crm/leads/crm-lead-get.md)
- deal [crm.deal.get](../../crm/deals/crm-deal-get.md)
- contact [crm.contact.get](../../crm/contacts/crm-contact-get.md)
- company [crm.company.get](../../crm/companies/crm-company-get.md)
- estimate [crm.quote.get](../../crm/quote/crm-quote-get.md)

In the case of embedding the widget in a custom object, the type identifier can be obtained from the value of the `PLACEMENT` parameter. In the example above, it is `183`.

||
|| **ASSOCIATED_ENTITY_ID***
[`string`](../../data-types.md) | Identifier of the CRM activity for which the widget was opened.

Can be used to retrieve additional information using the [crm.activity.get](../../crm/timeline/activities/crm-activity-get.md) method.

||
|| **ASSOCIATED_ENTITY_TYPE_ID***
[`string`](../../data-types.md) | Identifier of the CRM activity type for which the widget was opened.

||
|| **TYPE_ID***
[`string`](../../data-types.md) | _to be added later_

||
|| **TYPE_CATEGORY_ID***
[`string`](../../data-types.md) | _to be added later_

||
|| **TIMELINE_ITEM_ID***
[`string`](../../data-types.md) | _to be added later_

||
|#

## Continue Learning

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)