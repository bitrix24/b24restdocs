# Simplified Method for Obtaining OAuth 2.0 Tokens

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

An application that opens in a frame inside the Bitrix24 interface does not need to go through the [complete authorization protocol](index.md). Bitrix24 passes ready-made tokens itself every time the application opens.

The tokens are issued for the user who opened the application and are limited by their permissions in Bitrix24.

## What the Application Receives on Opening

Bitrix24 contacts the application address with a POST request: some parameters arrive in the query string of the address, the rest — in the request body.

```php
Array
(
    [DOMAIN] => portal.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => dd8cec11e347088fe87c44870a9f1dba
    [AUTH_ID] => ahodg4h37n89vo17gbkgq0x1l825nnb5
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 2lg086mxijlpvwh0h7r4nl19udm4try5
    [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
    [APPLICATION_TOKEN] => ec1b2074a9d3f5c81b6e40d27a95cf38
    [APPLICATION_SCOPE] => crm,entity,im,task
    [member_id] => a223c6b3710f85df22e9377d6c4f7553
    [status] => F
    [PLACEMENT] => DEFAULT
)
```

**Parameters in the Query String of the Application Address**

#|
|| **Parameter** | **Description** ||
|| **DOMAIN** | The address of the Bitrix24 where the application is open ||
|| **PROTOCOL** | The access protocol:

- `0` — HTTP
- `1` — HTTPS
||
|| **LANG** | The interface language of the user who opened the application. You can localize the application's own interface based on it ||
|| **APP_SID** | The application session identifier. Bitrix24 generates a new one each time the application is rendered and uses it to link the js library with the application environment ||
|#

**Parameters in the POST Request Body**

#|
|| **Parameter** | **Description** ||
|| **AUTH_ID** | The main authorization token for accessing the REST API. The same as `access_token` in the complete protocol ||
|| **AUTH_EXPIRES** | The lifetime of `AUTH_ID` in seconds ||
|| **REFRESH_ID** | The authorization renewal token. The same as `refresh_token` in the complete protocol ||
|| **SERVER_ENDPOINT** | The address of the authorization server that the application contacts for a new pair of tokens ||
|| **APPLICATION_TOKEN** | The application token. The handler can use it to verify that the request came from Bitrix24. The same value arrives in the `application_token` parameter for [event handlers](../../api-reference/events/safe-event-handlers.md) ||
|| **APPLICATION_SCOPE** | A comma-separated list of [scopes](../../api-reference/scopes/permissions.md) granted to the application ||
|| **member_id** | The unique identifier of Bitrix24, independent of the domain name ||
|| **status** | The status of the application:

- `L` — local application
- `F` — free mass-market application
- `D` — demo version of a mass-market application
- `T` — trial version of a mass-market application, time-limited
- `P` — paid mass-market application
||
|| **PLACEMENT** | The placement code. For the main application page it is `DEFAULT`. For [widgets](../../api-reference/widgets/index.md), the placement code and an additional `PLACEMENT_OPTIONS` parameter arrive ||
|#

{% note info %}

The `status` value is informational. To obtain a trusted status, call the [app.info](../../api-reference/common/system/app-info.md) method on the authorization server: `oauth.bitrix.info/rest/app.info`

{% endnote %}

## How to Use the Received Tokens

With the `AUTH_ID` value, you can call REST API methods right away — pass it in the `auth` parameter.

```bash
https://portal.bitrix24.com/rest/crm.deal.list?auth=ahodg4h37n89vo17gbkgq0x1l825nnb5
```

An application in a frame can also call methods on the browser side — through the [js library](../../sdk/bx24-js-sdk/index.md), using the [BX24.callMethod](../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-method.md) and [BX24.callBatch](../../sdk/bx24-js-sdk/how-to-call-rest-methods/bx24-call-batch.md) methods. The library substitutes the authorization itself.

`AUTH_ID` lives for one hour, so for background work without the user, retain `REFRESH_ID` — the application uses it to obtain a new pair of tokens, see [OAuth 2.0 Token Automatic Renewal](auto-renewal.md).

## Tokens on Application Installation

A separate installation script is specified in the settings of a local or mass-market application. It is shown to the user in a frame once, at the moment of installation, and receives the same data as a regular application page:

- [Installation Wizard for Local Application](../app-installation/local-apps/installation-master.md)
- [Installation Wizard for Mass-Market Application](../app-installation/mass-market-apps/installation-master.md)

Retain both tokens in the installation script, above all `REFRESH_ID` — then the application will be able to work with the REST API after the user closes the frame.

## Tokens for an Application Without an Interface

An application that works only through the API has no page in a frame, which means there is no moment when Bitrix24 passes the tokens on opening. Such an application receives the tokens at the handler specified in its settings: Bitrix24 contacts the handler immediately after the installation and passes the `auth` object with both tokens. The tokens are issued for the user who installed the application.

The data arrives in the body of a POST request in the `application/x-www-form-urlencoded` format, with nested objects as fields with square brackets. The handler reads them as ordinary form fields, and there is no need to parse JSON.

```php
$auth = $_POST['auth'] ?? [];
$refreshToken = $auth['refresh_token'] ?? null;
```

Event handlers usually do not receive `refresh_token`. The `ONAPPINSTALL` event is an exception: access can be renewed only with the token from this event.

How to set up the handler and what to retain in it is described in the articles on the installation callback — for a [local](../app-installation/local-apps/installation-callback.md) and for a [mass-market](../app-installation/mass-market-apps/installation-callback.md) application. The composition of the request data is covered on the page of the [OnAppInstall](../../api-reference/common/events/on-app-install.md) event.

{% note warning %}

The event may arrive with a delay, so it is unreliable as the only source of tokens. If you need the tokens immediately after the installation, duplicate their retrieval using one of the methods above.

{% endnote %}

## What to Do Next

- renew access with the retained `REFRESH_ID` — [OAuth 2.0 Token Automatic Renewal](auto-renewal.md)
- obtain tokens for an application outside the Bitrix24 interface — [Complete OAuth 2.0 Authorization Protocol](index.md)
- call a method with the received token — [How to Call REST API Methods](../how-to-call-rest-api/index.md)