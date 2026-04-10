# Collection of IM_SMILES_SELECTOR Emojis

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning " " %}
  
**DEPRECATED**

The `IM_SMILES_SELECTOR` embedding has been deprecated since version `im 25.1600.0`. Emojis have been replaced with [stickers](https://helpdesk.bitrix24.com/open/25866875/).

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)

This page describes the deprecated embedding for enhancing the capabilities of emojis and Giphy. Use it only as archival reference.

The embedding code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The embedding does not appear in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

#|
|| **Embedding Code** | **Location** ||
|| `IM_SMILES_SELECTOR` | Enhances the capabilities of emojis and Giphy ||
|#

### Embedding Status

`IM_SMILES_SELECTOR` is described for compatibility with older versions of the `im` module. For new integrations, use the current embedding points from the [section overview](./index.md).

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

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
[`string`](../../data-types.md) | The Bitrix24 address from which the widget handler was called ||
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
[`string`](../../data-types.md) | Unique string identifier of Bitrix24 from which the widget handler was called.  ||
|| **status**
[`string`](../../data-types.md) | Type of the application that registered the handler for this widget. Accepts values:

- `L` - [local](../../../local-integrations/local-apps.md) application
- `F` - [free mass-market](../../../market/index.md) application
||
|| **PLACEMENT***
[`string`](../../data-types.md) | Code of the widget embedding location. You can use the same handler URL for all your widgets. The value that Bitrix24 will report in the `PLACEMENT` parameter will help determine from which specific widget embedding location your handler was called in each case ||
|| **PLACEMENT_OPTIONS**
[`string`](../../data-types.md) | Additional data in the form of a JSON string defining the context of the widget execution. In this case, it is an array containing the numeric identifier of the CRM element in the card where the widget handler was called. The `PLACEMENT_OPTIONS` parameter, along with the `PLACEMENT` parameter, allows you to accurately determine for which specific CRM object the widget handler was called ||
|#

## Continue Your Study

- [{#T}](./index.md)
- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../bx24-widget-methods.md)
- [{#T}](../../../settings/interactivity/index.md)