# Widget Above the IM_TEXTAREA Message

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- What the handler receives (copied from Sergey's example, detail-tab.md)
- Typical use-cases and scenarios — need to add if there's anything
- Continue exploring (copied from Sergey's example, detail-tab.md)
- no screenshot

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)

You can add your item to the panel above the input field (content generation while composing a message).

The specific placement code for the widget is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

## Where the Widget is Embedded

#|
|| **Widget Code** | **Location** ||
|| `IM_TEXTAREA` | Item in the panel above the input field  ||
|#

## What the Handler Receives

Data is sent as a POST request {.b24-info}

```js
'DOMAIN': 'xxx.bitrix24.com'
'PROTOCOL': 1
'LANG': 'en'
'APP_SID': '99c80eff6378726287350416ee5fef0'
'AUTH_ID': '6061e72600631fcd00005a4b00000001f0f1076700000000f69dd5fc643d9ce2fdbc1'
'AUTH_EXPIRES': 3600
'REFRESH_ID': '50e00aa340631fcd00005a4b00000001f0f1071111116580a5b83c2de639ef28c12'
'member_id': 'da45a03b265ed12127f8a258d793cc5d'
'status': 'L'
'PLACEMENT': 'CRM_DEAL_DETAIL_TAB'
'PLACEMENT_OPTIONS': '{"ID":"3443"}'
```

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

## Continue Exploring

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)