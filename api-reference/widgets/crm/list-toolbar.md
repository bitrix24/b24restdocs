# Dropdown Menu Item Above the CRM Entity List CRM_XXX_LIST_TOOLBAR, CRM_DYNAMIC_XXX_LIST_TOOLBAR

> Scope: [`crm`](../../scopes/permissions.md)

You can add your dropdown menu item above the list of CRM entity objects: [leads](../../crm/leads/index.md), [contacts](../../crm/contacts/index.md), [companies](../../crm/companies/index.md), [deals](../../crm/deals/index.md), [old invoices](../../crm/outdated/invoice/index.md), [estimates](../../crm/quote/index.md), [new invoices](../../crm/universal/invoice.md), [custom entity types](../../crm/universal/index.md).

![Widget as a Context Menu Item in Deal](./_images/CRM__LIST_TOOLBAR.png "Widget as a Context Menu Item in Deal")

The specific widget placement code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The widget will not be displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

#|
|| **Widget Code** | **Location** ||
|| `CRM_LEAD_LIST_TOOLBAR` | Dropdown menu item above the list of [leads](../../crm/leads/index.md) ||
|| `CRM_CONTACT_LIST_TOOLBAR` | Dropdown menu item above the list of [contacts](../../crm/contacts/index.md) ||
|| `CRM_COMPANY_LIST_TOOLBAR` | Dropdown menu item above the list of [companies](../../crm/companies/index.md) ||
|| `CRM_DEAL_LIST_TOOLBAR` | Dropdown menu item above the list of [deals](../../crm/deals/index.md) ||
|| `CRM_INVOICE_LIST_TOOLBAR` | Dropdown menu item above the list of [old invoices](../../crm/outdated/invoice/index.md) ||
|| `CRM_SMART_INVOICE_LIST_TOOLBAR` | Dropdown menu item above the list of [new invoices](../../crm/universal/invoice.md) ||
|| `CRM_QUOTE_LIST_TOOLBAR` | Dropdown menu item above the list of [estimates](../../crm/quote/index.md) ||
|| `CRM_DYNAMIC_XXX_LIST_TOOLBAR` | Dropdown menu item above the list of custom CRM entity type items. Instead of XXX, you need to specify the numeric identifier of the specific [custom entity type](../../crm/universal/index.md). For example, `CRM_DYNAMIC_183_LIST_TOOLBAR` ||
|#

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

{% list tabs %}

- CRM_LEAD_LIST_TOOLBAR

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => com
        [APP_SID] => 17621e81b6c5e43e706be4f943719513
        [AUTH_ID] => b2f19f6600631fcd00005a4b00000001f0f1071894b660abb19a2fa0362714239a2aaa
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => a270c76600631fcd00005a4b00000001f0f107a47747d2035445dbcaa0886ec97678df
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_LEAD_LIST_TOOLBAR
    )

    ```

- CRM_DEAL_LIST_TOOLBAR

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => com
        [APP_SID] => 55fb79c4a1bb3645c8bf3b5f0cfca12f
        [AUTH_ID] => 31f29f6600631fcd00005a4b00000001f0f10781afcd4e67da98de2c0c3ba491e6d6f5
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 2171c76600631fcd00005a4b00000001f0f10731ca47c52d032bf3568e3f94c3d9750a
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_DEAL_LIST_TOOLBAR
    )

    ```

- CRM_CONTACT_LIST_TOOLBAR

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => com
        [APP_SID] => 3aec6e81c200862ebe7ed02c5a0551d9
        [AUTH_ID] => 4af29f6600631fcd00005a4b00000001f0f107657b02e0d0eaaaabbe09ea6c8628110d
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 3a71c76600631fcd00005a4b00000001f0f107ec7126f6c7499958546207d42d820184
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_CONTACT_LIST_TOOLBAR
    )

    ```

- CRM_COMPANY_LIST_TOOLBAR

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => com
        [APP_SID] => 179765226ec81db398cc98e4a5e9015e
        [AUTH_ID] => 5ff29f6600631fcd00005a4b00000001f0f10706443c53e3994101a662e9b245ee398e
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 4f71c76600631fcd00005a4b00000001f0f10787f7352bf08be012b32c362e6c808f72
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_COMPANY_LIST_TOOLBAR
    )
    ```

- CRM_INVOICE_LIST_TOOLBAR

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => com
        [APP_SID] => 611bd605715c4de60c7efe1fc82ce0be
        [AUTH_ID] => 79f29f6600631fcd00005a4b00000001f0f107e0bf261552367a5d567964f8862976b1
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 6971c76600631fcd00005a4b00000001f0f107f5b4499d2f41d14ec3142fb9b189b409
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_INVOICE_LIST_TOOLBAR
    )
    
    ```

- CRM_SMART_INVOICE_LIST_TOOLBAR

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 1dc4a02fd9c7c094bb78cac8689d23cb
        [AUTH_ID] => 6986d4ca670076a4b8006f518000000001201c07456529898882f844c5d744f564bcfafb
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 7653f2670076a4b8006f518000000001201c0710663db8587fccc71874c46996bf6f49
        [member_id] => e8857f161a1a8288f312b6cc6ad67995
        [status] => L
        [PLACEMENT] => CRM_SMART_INVOICE_LIST_TOOLBAR
    )
    
    ```

- CRM_QUOTE_LIST_TOOLBAR

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => com
        [APP_SID] => 5389d2aee1d75061a59be00996972f78
        [AUTH_ID] => 8ef29f6600631fcd00005a4b00000001f0f107f56f228b134e9f88dd8088ce08d9de0e
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 7e71c76600631fcd00005a4b00000001f0f107515f3cc004a6876f039fab870a2cbdc2
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_QUOTE_LIST_TOOLBAR
    )
    
    ```

- CRM_DYNAMIC_XXX_LIST_TOOLBAR

    ```php

    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => com
        [APP_SID] => 30b1cd2ce933551b37c441f8bafc5545
        [AUTH_ID] => a9f29f6600631fcd00005a4b00000001f0f107f69952670946852790cb3ec5bd1ab2e9
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 9971c76600631fcd00005a4b00000001f0f1075c07a22a5dc9d29f124040e460ac04b9
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => CRM_DYNAMIC_183_LIST_TOOLBAR
    )
    
    ```

{% endlist %}

{% include [Footnote on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

In the current widget, the `PLACEMENT_OPTIONS` parameter is not passed.

## Continue Learning

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../../settings/interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)