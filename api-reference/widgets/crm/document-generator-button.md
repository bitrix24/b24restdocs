# Dropdown Menu Item for the Document Generator CRM_XXX_DOCUMENTGENERATOR_BUTTON

> Scope: [`intranet`](../../scopes/permissions.md)

You can add your item to the dropdown menu of the document generator for CRM entities: [leads](../../crm/leads/index.md), [contacts](../../crm/contacts/index.md), [companies](../../crm/companies/index.md), [deals](../../crm/deals/index.md), [estimates](../../crm/quote/index.md), [new invoices](../../crm/universal/invoice.md), [custom entity types](../../crm/universal/index.md).

The specific widget placement code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The widget will not be displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

#|
|| **Widget Code** | **Location** ||
|| `CRM_LEAD_DOCUMENTGENERATOR_BUTTON` | Item in the dropdown menu of the document generator for [leads](../../crm/leads/index.md) ||
|| `CRM_CONTACT_DOCUMENTGENERATOR_BUTTON` | Item in the dropdown menu of the document generator for [contacts](../../crm/contacts/index.md) ||
|| `CRM_COMPANY_DOCUMENTGENERATOR_BUTTON` | Item in the dropdown menu of the document generator for [companies](../../crm/companies/index.md) ||
|| `CRM_DEAL_DOCUMENTGENERATOR_BUTTON` | Item in the dropdown menu of the document generator for [deals](../../crm/deals/index.md) ||
|| `CRM_SMART_INVOICE_DOCUMENTGENERATOR_BUTTON` | Item in the dropdown menu of the document generator for [invoices](../../crm/universal/invoice.md) ||
|| `CRM_QUOTE_DOCUMENTGENERATOR_BUTTON` | Item in the dropdown menu of the document generator for [estimates](../../crm/quote/index.md) ||
|| `CRM_DYNAMIC_XXX_DOCUMENTGENERATOR_BUTTON` | Item in the dropdown menu of the document generator for custom CRM entity types. Instead of XXX, specify the numeric identifier of the specific [custom entity type](../../crm/universal/index.md). For example, `CRM_DYNAMIC_183_DOCUMENTGENERATOR_BUTTON` ||
|#

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

{% list tabs %}

- CRM_LEAD_DOCUMENTGENERATOR_BUTTON

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 459ca665c9cab4ddfe9ea5ae2a0840f1
        [AUTH_ID] => d54bba6600631fcd00005a4b00000001f0f10778a1773b649abec1feea11861a78c85a
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => c5cae16600631fcd00005a4b00000001f0f1070bb92bc9d139bcccf13fd5061e168c97
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_LEAD_DOCUMENTGENERATOR_BUTTON
        [PLACEMENT_OPTIONS] => {"ENTITY_ID":"6591"}
    )

    ```

- CRM_DEAL_DOCUMENTGENERATOR_BUTTON

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 23ab3058338edf7c4c30a52e4d0b3f94
        [AUTH_ID] => 024cba6600631fcd00005a4b00000001f0f10776117251d4c2d883c3b981273ddc1d2f
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => f2cae16600631fcd00005a4b00000001f0f1071ccf5fc07f1f6e09595a46e2689ad63b
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_DEAL_DOCUMENTGENERATOR_BUTTON
        [PLACEMENT_OPTIONS] => {"ENTITY_ID":"3473"}
    )

    ```

- CRM_CONTACT_DOCUMENTGENERATOR_BUTTON

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 8610c737a7b6d9e8707e897310c92a5c
        [AUTH_ID] => 234cba6600631fcd00005a4b00000001f0f107e5d9a6b11c75b358354b7909a3cbb1d5
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 13cbe16600631fcd00005a4b00000001f0f1072808b892db51d0797e9f6ef1f47ac479
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_CONTACT_DOCUMENTGENERATOR_BUTTON
        [PLACEMENT_OPTIONS] => {"ENTITY_ID":"13037"}
    )

    ```

- CRM_COMPANY_DOCUMENTGENERATOR_BUTTON

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 6c69387b1893adfbce7b0e9dd09a4334
        [AUTH_ID] => 474cba6600631fcd00005a4b00000001f0f1075e5c6675d9cc38d0226e8d64a137f0d4
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 37cbe16600631fcd00005a4b00000001f0f107ad057bdb52baf3c9f25ebd88c351e5cb
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_COMPANY_DOCUMENTGENERATOR_BUTTON
        [PLACEMENT_OPTIONS] => {"ENTITY_ID":"2946"}
    )
        
    ```

- CRM_QUOTE_DOCUMENTGENERATOR_BUTTON

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 92520ef005add4cddf34ce7d56417cd6
        [AUTH_ID] => 734cba6600631fcd00005a4b00000001f0f10783d784554c5d20e60a70194e7fae0928
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 63cbe16600631fcd00005a4b00000001f0f107f7e3fca578b3e3b68e5be0cf70b35ccc
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_QUOTE_DOCUMENTGENERATOR_BUTTON
        [PLACEMENT_OPTIONS] => {"ENTITY_ID":"5"}
    )
    
    ```

- CRM_SMART_INVOICE_DOCUMENTGENERATOR_BUTTON

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
        [PLACEMENT] => CRM_SMART_INVOICE_DOCUMENTGENERATOR_BUTTON
        [PLACEMENT_OPTIONS] => {"ENTITY_ID":"32"}
    )
    
    ```

- CRM_DYNAMIC_XXX_DOCUMENTGENERATOR_BUTTON

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => e704d5033456989068deea36a324b888
        [AUTH_ID] => 904cba6600631fcd00005a4b00000001f0f1072f641be14b9ac7606506e5e3ed850130
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 80cbe16600631fcd00005a4b00000001f0f107151fd45dc66ee22283e42cdf941e8436
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_DYNAMIC_183_DOCUMENTGENERATOR_BUTTON
        [PLACEMENT_OPTIONS] => {"ENTITY_ID":"3"}
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
|| **ENTITY_ID*** 
[`string`](../../data-types.md) | Identifier of the CRM entity for which the widget was opened.

It can be used to retrieve additional information using the corresponding methods:

- any entity type [crm.item.get](../../crm/universal/crm-item-get.md) specifying entityTypeId = '1' for leads, '2' for deals, and [etc.](../../crm/data-types.md#object_type)
- lead [crm.lead.get](../../crm/leads/crm-lead-get.md)
- deal [crm.deal.get](../../crm/deals/crm-deal-get.md)
- contact [crm.contact.get](../../crm/contacts/crm-contact-get.md)
- company [crm.company.get](../../crm/companies/crm-company-get.md)
- estimate [crm.quote.get](../../crm/quote/crm-quote-get.md)

In the case of embedding the widget in a custom entity type, the type identifier can be obtained from the value of the `PLACEMENT` parameter. In the example above — `183`

||
|#

## Continue Learning

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../../settings/interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)