# Menu Item in the Site Settings LANDING_SETTINGS

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- What the handler receives (copied from Sergey's example, detail-tab.md)
- Typical use-cases and scenarios — need to add if there is anything
- Continue your study (copied from Sergey's example, detail-tab.md)
- No screenshot

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)

You can add your own menu item in the site settings.

The specific widget placement code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

## Where the Widget is Embedded

#| 
|| **Widget Code** | **Location** ||
|| `LANDING_SETTINGS` | Menu item in the site settings ||
|#

## What the Handler Receives

Data is transmitted in the form of a POST request {.b24-info}

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

#| 
|| **Parameter**
`type` | **Description** ||
|| **DOMAIN*** 
[`string`](../../data-types.md) | The address of Bitrix24 where the widget handler was called ||
|| **PROTOCOL*** 
[`string`](../../data-types.md) | Secure or non-secure HTTP protocol:

- `0` - HTTP
- `1` - HTTPS
 ||
|| **LANG*** 
[`string`](../../data-types.md) | The user interface language of Bitrix24 that called the widget. You can localize the interface language in your widget based on this value ||
|| **APP_SID** 
[`string`](../../data-types.md) | String identifier of the application that registered the widget handler ||
|| **AUTH_ID** 
[`string`](../../data-types.md) | Authorization token [OAuth 2](../../../settings/oauth/simple-way.md) issued for the user who called the widget. Can be used for REST API calls on behalf of this user ||
|| **AUTH_EXPIRES** 
[`integer`](../../data-types.md) | Time in seconds after which the authorization token will become invalid ||
|| **REFRESH_ID** 
[`string`](../../data-types.md) | Refresh token [OAuth 2](../../../settings/oauth/simple-way.md) issued for the user who called the widget. Can be used to refresh the authorization token on behalf of this user ||
|| **member_id*** 
[`string`](../../data-types.md) | Unique string identifier of Bitrix24 where the widget handler was called. ||
|| **status** 
[`string`](../../data-types.md) | Type of the application that registered the handler for this widget. Accepts values:

- `L` - [local](../../../local-integrations/local-apps.md) application
- `F` - [free mass-market](../../../market/index.md) application
||
|| **PLACEMENT*** 
[`string`](../../data-types.md) | Code of the widget placement. You can use the same handler URL for all your widgets. The value that Bitrix24 will report in the `PLACEMENT` parameter will help determine from which specific widget placement your handler was called in each case ||
|| **PLACEMENT_OPTIONS** 
[`string`](../../data-types.md) | Additional data in the form of a JSON string defining the context of the widget execution. In this case, it is an array containing the numeric identifier of the CRM element in the detail form where the widget handler was called. The `PLACEMENT_OPTIONS` parameter along with the `PLACEMENT` parameter allows you to accurately determine for which specific CRM object the widget handler was called ||
|#

## Continue Your Study

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../../settings/interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)