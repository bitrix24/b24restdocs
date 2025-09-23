# Button above the timeline of the CRM_XXX_DETAIL_ACTIVITY, CRM_DYNAMIC_XXX_DETAIL_ACTIVITY card

> Scope: [`crm`](../../scopes/permissions.md)

You can add your item to the timeline menu of CRM objects: [leads](../../crm/leads/index.md), [contacts](../../crm/contacts/index.md), [companies](../../crm/companies/index.md), [deals](../../crm/deals/index.md), [estimates](../../crm/quote/index.md), [new invoices](../../crm/universal/invoice.md), [custom object types](../../crm/universal/index.md).

The specific placement code for the widget is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

Extended capabilities of the button above the timeline are described in the article [Additional integration capabilities in CRM_XXX_DETAIL_ACTIVITY](./detail-activity-area.md)

## Where the widget is integrated

#|
|| **Widget Code** | **Location** ||
|| `CRM_LEAD_DETAIL_ACTIVITY` | Timeline menu item for [lead](../../crm/leads/index.md) ||
|| `CRM_CONTACT_DETAIL_ACTIVITY` | Timeline menu item for [contact](../../crm/contacts/index.md) ||
|| `CRM_COMPANY_DETAIL_ACTIVITY` | Timeline menu item for [company](../../crm/companies/index.md) ||
|| `CRM_DEAL_DETAIL_ACTIVITY` | Timeline menu item for [deal](../../crm/deals/index.md) ||
|| `CRM_QUOTE_DETAIL_ACTIVITY` | Timeline menu item for [estimate](../../crm/quote/index.md) ||
|| `CRM_SMART_INVOICE_DETAIL_ACTIVITY` | Timeline menu item for [invoices](../../crm/universal/invoice.md) ||
|| `CRM_DYNAMIC_XXX_DETAIL_ACTIVITY` | Timeline menu item for custom CRM object type. Instead of XXX, you need to specify the numeric identifier of the specific [custom object type](../../crm/universal/index.md). For example, `CRM_DYNAMIC_183_DETAIL_ACTIVITY` ||
|#

## What the handler receives

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

- CRM_SMART_INVOICE_DETAIL_ACTIVITY

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => adada92053b22a4de3895402a01693cf
        [AUTH_ID] => 69c7ca670076a4b8006f518000000001201c0720c9c9d78077b5f2c5530f64b061c8a1
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 5946f2670076a4b8006f518000000001201c07709da4b12d3c7e82e120a20e547b638f
        [member_id] => e8857f161a1a8288f312b6cc6ad67995
        [status] => L
        [PLACEMENT] => CRM_SMART_INVOICE_DETAIL_ACTIVITY
        [PLACEMENT_OPTIONS] => {"ID":"32"}
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

{% include [Footnote about required parameters](../../../_includes/required.md) %}

{% include notitle [description of standard data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is a JSON string containing an array of one or more keys.

{% include [Footnote about required parameters](../../../_includes/required.md) %}

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

In the case of integrating the widget into a custom object, the type identifier can be obtained from the value of the `PLACEMENT` parameter. In the example above — `183`

||
|#

## Continue exploring

- [{#T}](./detail-activity-area.md)
- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../../settings/interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)