# Button Above the Timeline of the CRM Entity Card CRM_XXX_DETAIL_ACTIVITY, CRM_DYNAMIC_XXX_DETAIL_ACTIVITY

> Scope: [`crm`](../../scopes/permissions.md)

You can add your item to the timeline menu of CRM objects such as [leads](../../crm/leads/), [contacts](../../crm/contacts/), [companies](../../crm/companies/), [deals](../../crm/deals/) and [custom types](../../crm/universal/) of objects.

The specific placement code for the widget is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

## Where the Widget is Embedded

#|
|| **Placement Code** | **Location** ||
|| `CRM_LEAD_DETAIL_ACTIVITY` | Timeline menu item for [lead](../../crm/leads/) ||
|| `CRM_CONTACT_DETAIL_ACTIVITY` | Timeline menu item for [contact](../../crm/contacts/) ||
|| `CRM_COMPANY_DETAIL_ACTIVITY` | Timeline menu item for [company](../../crm/companies/) ||
|| `CRM_DEAL_DETAIL_ACTIVITY` | Timeline menu item for [deal](../../crm/deals/) ||
|| `CRM_QUOTE_DETAIL_ACTIVITY` | Timeline menu item for [estimate](../../crm/quote/) ||
|| `CRM_DYNAMIC_XXX_DETAIL_ACTIVITY` | Timeline menu item for custom type CRM objects. Instead of XXX, you need to specify the numeric identifier of the specific [custom type of objects](../../crm/universal/). For example, `CRM_DYNAMIC_183_DETAIL_ACTIVITY` ||
|#

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

{% list tabs %}

- CRM_LEAD_DETAIL_ACTIVITY

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 231dec2797809e63f2183cd9e5c1db79
        [AUTH_ID] => 29d1a06600631fcd00005a4b00000001f0f1071497cc09c28ec609a43bb0c802d2ad41
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 1950c86600631fcd00005a4b00000001f0f107804dc35e52c3002c7e7e155337b89e25
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_LEAD_DETAIL_ACTIVITY
        [PLACEMENT_OPTIONS] => {"ID":"6591"}
    )

    ```

- CRM_DEAL_DETAIL_ACTIVITY

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => fb219fe14b3bb927487324b3bf561e3a
        [AUTH_ID] => 4ad1a06600631fcd00005a4b00000001f0f107e9193b10f6ec5579451a015c78a66829
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 3a50c86600631fcd00005a4b00000001f0f107d9898b9feec5e1fd2e9cf59d40121087
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_DEAL_DETAIL_ACTIVITY
        [PLACEMENT_OPTIONS] => {"ID":"3473"}
    )

    ```

- CRM_CONTACT_DETAIL_ACTIVITY

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => a3e8af64160b5eaa51e16d4cc14f23dc
        [AUTH_ID] => 65d1a06600631fcd00005a4b00000001f0f107635565dfe5bc6e1924790e68da8091f7
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 5550c86600631fcd00005a4b00000001f0f10715d6275f1e55e90b58fa5444cc00efdf
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_CONTACT_DETAIL_ACTIVITY
        [PLACEMENT_OPTIONS] => {"ID":"13037"}
    )

    ```

- CRM_COMPANY_DETAIL_ACTIVITY

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => c718482c9df48a3115d039967d5183a6
        [AUTH_ID] => 8dd1a06600631fcd00005a4b00000001f0f10753abde956bcff8eea8801f6ae598becc
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 7d50c86600631fcd00005a4b00000001f0f1079035d2a1022c0def3c60824ec692788b
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_COMPANY_DETAIL_ACTIVITY
        [PLACEMENT_OPTIONS] => {"ID":"2946"}
    )
        
    ```

- CRM_QUOTE_DETAIL_ACTIVITY

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 7e776f9d59d8346b452bd9b60a8f3925
        [AUTH_ID] => b4d1a06600631fcd00005a4b00000001f0f107d6fc31ffbd4740c5a0a5393c8744ac8a
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => a450c86600631fcd00005a4b00000001f0f10755a4090ea60da33ae27abd087bded527
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_QUOTE_DETAIL_ACTIVITY
        [PLACEMENT_OPTIONS] => {"ID":"5"}
    )
    
    ```

- CRM_DYNAMIC_XXX_DETAIL_ACTIVITY

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => e1f8a9f4be2f07b8645216f7b3104a20
        [AUTH_ID] => e1d1a06600631fcd00005a4b00000001f0f1077ebff8b4fdddb2a57ccdbc1edd9ce1cf
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => d150c86600631fcd00005a4b00000001f0f1070a03cba852bafb9c58de5ea9fe9a0daa
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_DYNAMIC_183_DETAIL_ACTIVITY
        [PLACEMENT_OPTIONS] => {"ID":"3"}
    )
    
    ```

{% endlist %}

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is a JSON string containing an array of one or more keys.

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Parameter** | **Description** ||
|| **ID***
[`string`](../../data-types.md) | Identifier of the CRM object for which the widget was opened.

It can be used to retrieve additional information using the corresponding methods:

- any object type [crm.item.get](../../crm/universal/crm-item-get.md) specifying entityTypeId = '1' for leads, '2' for deals, and [etc.](../../crm/data-types.md#object_type)
- lead [crm.lead.get](../../crm/leads/crm-lead-get.md)
- deal [crm.deal.get](../../crm/deals/crm-deal-get.md)
- contact [crm.contact.get](../../crm/contacts/crm-contact-get.md)
- company [crm.company.get](../../crm/companies/crm-company-get.md)
- estimate [crm.quote.get](../../crm/quote/crm-quote-get.md)
 
In the case of embedding the widget in a custom type object, the type identifier can be obtained from the value of the `PLACEMENT` parameter. In the example above, it is `183`.

||
|#

## Continue Learning

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)