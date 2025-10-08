# Tab in the detail form of the CRM entity CRM_XXX_DETAIL_TAB, CRM_DYNAMIC_XXX_DETAIL_TAB

> Scope: [`crm`](../../scopes/permissions.md)

You can add your own tabs to the detail form of CRM objects: [leads](../../crm/leads/index.md), [contacts](../../crm/contacts/index.md), [companies](../../crm/companies/index.md), [deals](../../crm/deals/index.md), [estimates](../../crm/quote/index.md), [new invoices](../../crm/universal/invoice.md), [custom object types](../../crm/universal/index.md).

![Widget as a tab in the detail form of the CRM entity](./_images/CRM_DEAL_DETAIL_TAB.png "Widget as a tab in the detail form of the CRM entity")

The specific placement code for the widget is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The widget will not be displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the widget is embedded

#|
|| **Placement Code** | **Location** ||
|| `CRM_LEAD_DETAIL_TAB` | Tab in the [lead](../../crm/leads/index.md) detail form ||
|| `CRM_DEAL_DETAIL_TAB` | Tab in the [deal](../../crm/deals/index.md) detail form ||
|| `CRM_CONTACT_DETAIL_TAB` | Tab in the [contact](../../crm/contacts/index.md) detail form ||
|| `CRM_COMPANY_DETAIL_TAB` | Tab in the [company](../../crm/companies/index.md) detail form ||
|| `CRM_QUOTE_DETAIL_TAB` | Tab in the [estimate](../../crm/quote/index.md) detail form ||
|| `CRM_SMART_INVOICE_DETAIL_TAB` | Tab in the [invoice](../../crm/universal/invoice.md) detail form ||
|| `CRM_DYNAMIC_XXX_DETAIL_TAB` | Tab in the detail form of a custom object type in CRM. Instead of XXX, specify the numeric identifier of the specific [custom object type](../../crm/universal/index.md). For example, `CRM_DYNAMIC_183_DETAIL_TAB` ||
|#

## What the handler receives

Data is transmitted as a POST request {.b24-info}

{% list tabs %}

- CRM_LEAD_DETAIL_TAB

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 32dc7a69a3dac11ea9c95dfc6bf5dd8a
        [AUTH_ID] => 1bf49f6600631fcd00005a4b00000001f0f107e9a9ddb6de2bd5f7856ac587b492adb4
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 0b73c76600631fcd00005a4b00000001f0f1079fd883d9c43bf4abf545709c61eb8f69
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_LEAD_DETAIL_TAB
        [PLACEMENT_OPTIONS] => {"ID":"6591"}
    )

    ```

- CRM_DEAL_DETAIL_TAB

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => d8286e173e919aa1695254997a6e3123
        [AUTH_ID] => 3cf49f6600631fcd00005a4b00000001f0f107d9825065d14b0d269c63cdaa0bb1967d
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 2c73c76600631fcd00005a4b00000001f0f1076f22983f060e8e14120e47cbc2c227a0
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_DEAL_DETAIL_TAB
        [PLACEMENT_OPTIONS] => {"ID":"3473"}
    )

    ```

- CRM_CONTACT_DETAIL_TAB

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => d17a24f20960a2971eda0e69754e62a2
        [AUTH_ID] => 57f49f6600631fcd00005a4b00000001f0f1077ef2d8b6c37097b8985bb7fb4948d1e8
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 4773c76600631fcd00005a4b00000001f0f10711f2f134f53a44072e44b61677961fac
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_CONTACT_DETAIL_TAB
        [PLACEMENT_OPTIONS] => {"ID":"13037"}
    )

    ```

- CRM_COMPANY_DETAIL_TAB

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 1ff08edeb6a06f8f35a28fd745039801
        [AUTH_ID] => 74f49f6600631fcd00005a4b00000001f0f1070281f446cf788ea6bd54f8420750aaea
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 6473c76600631fcd00005a4b00000001f0f107b5ee25f08705b5f616a23e2130eb7fad
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_COMPANY_DETAIL_TAB
        [PLACEMENT_OPTIONS] => {"ID":"2946"}
    )
        
    ```

- CRM_QUOTE_DETAIL_TAB

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 5571169e21118b274f4fa57b8fd4e2b3
        [AUTH_ID] => aef49f6600631fcd00005a4b00000001f0f1079f066b85d07bc74dc9f4372d83152d70
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 9e73c76600631fcd00005a4b00000001f0f107541600cc2176b7d270db8ab3f1eecfcf
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_QUOTE_DETAIL_TAB
        [PLACEMENT_OPTIONS] => {"ID":"5"}
    )
    
    ```

- CRM_SMART_INVOICE_DETAIL_TAB

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => fff172819907af99a29b4830304aabe7
        [AUTH_ID] => ccbfca670076a4b8006f518000000001201c07b80ac830a875756c6c0c9073bec005c5
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => bc3ef2670076a4b8006f518000000001201c07efcbf35af9b89bb15ea3ab8e7223fe49
        [member_id] => e8857f161a1a8288f312b6cc6ad67995
        [status] => L
        [PLACEMENT] => CRM_SMART_INVOICE_DETAIL_TAB
        [PLACEMENT_OPTIONS] => {"ID":"32"}
    )
    
    ```

- CRM_DYNAMIC_XXX_DETAIL_TAB

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 5d7bde16d7895ef326320f00c4bfbd8d
        [AUTH_ID] => def49f6600631fcd00005a4b00000001f0f10700d7b3563b156732e94917116f0a81a1
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => ce73c76600631fcd00005a4b00000001f0f1072e9cc05d2796e9b91abaa262fc98bdf9
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_DYNAMIC_183_DETAIL_TAB
        [PLACEMENT_OPTIONS] => {"ID":"3"}
    )
    
    ```

{% endlist %}

{% include [Note on required parameters](../../../_includes/required.md) %}

{% include notitle [description of standard data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is a JSON string containing an array of one or more keys.

{% include [Note on required parameters](../../../_includes/required.md) %}

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
 
In the case of embedding the widget in a custom object, the type identifier can be obtained from the value of the `PLACEMENT` parameter. In the example above — `183`

||
|#

{% note tip "Typical use-cases and scenarios" %}

- [{#T}](../../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## Continue exploring

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../../settings/interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)