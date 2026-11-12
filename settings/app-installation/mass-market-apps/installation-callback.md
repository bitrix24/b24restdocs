# Installation Callback

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

An application without an interface still needs authorization tokens. The installation callback is the way to obtain them: Bitrix24 sends the tokens to your handler right after a user installs a mass-market application.

## When to Use It

The option suits applications that do not have their own page in Bitrix24. In the card of such an application, the *Add your page and item to the main menu* option is left unchecked. No item appears in the left menu, but the application can register widgets in embedding locations. Typically, the logic of such an application runs in event handlers.

The handler URL is specified in the application card, in the *Installation event handler URL* field. Bitrix24 accepts an HTTP or HTTPS URL. Use HTTPS: tokens are sent in the request.

If the user needs to configure something during installation, the callback is not a good fit: it has no interface. Choose the [Installation Wizard](./installation-master.md) for an application with an interface, or the [Configuration Wizard for REST-Only Applications](./rest-only-installation-master.md). All installation methods are compared in the overview [{#T}](./index.md).

## How It Works

Bitrix24 registers the address from the card as an event handler and sends a standard POST request to it. In PHP, the request fields are available in `$_POST`: `event`, `data`, `ts`, and the `auth` object with OAuth 2.0 authorization data. Typically, `auth` contains the access token, the refresh token, and the access token lifetime, but the request may contain no tokens.

Two events arrive at the same address:

#|
|| **Event** | **When It Arrives** | **What the Application Receives** ||
|| [`ONAPPINSTALL`](../../../api-reference/common/events/on-app-install.md) | On every installation and update of an application without its own page in Bitrix24 | Authorization of the employee who installed the application ||
|| [`ONAPPUSERREADY`](../../../api-reference/common/events/on-app-user-ready.md) | When the application system user is created or re-activated | Long-lived authorization of the application system user ||
|#

Distinguish the events by the `event` field. Otherwise, the application will treat the second event as a repeated installation.

The authorization data of the events is located in different parts of the request:

- `ONAPPINSTALL` — in `auth`
- `ONAPPUSERREADY` — the system user's in `data`, the authorization of the employee who installed the application in `auth`

The `application_token` used to verify authenticity is present in `auth` for both events.

An example of the `ONAPPINSTALL` request, with abbreviated `data` and `auth`:

```php
$_POST = [
  'event' => 'ONAPPINSTALL',
  'event_handler_id' => '17',
  'data' => [
    // event fields
  ],
  'ts' => '1696527000',
  'auth' => [
    'access_token' => '***',
    'refresh_token' => '***',
    'expires_in' => 3600,
    'scope' => 'crm,user',
    'domain' => 'some-domain.bitrix24.com',
    'client_endpoint' => 'https://some-domain.bitrix24.com/rest/',
    'server_endpoint' => 'https://oauth.bitrix.info/rest/',
    'member_id' => '***',
    'application_token' => '***'
  ]
];
```

The full set of fields is described on the event pages.

## What to Return

Return code 200. The installation does not depend on the response: for an application without an interface, it is considered complete as soon as the application is added. Store the tokens on your side. The access token has a limited lifetime and is renewed with the refresh token.

The [BX24.installFinish()](../../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md) method does not need to be called in this scenario, unlike in the [Installation Wizard](./installation-master.md). This is a method of the JS library, and the library works only within the frame of the application interface. The handler is invoked by the Bitrix24 server, and no browser is involved.

The handler is available at a public address, so anyone can send a request to it. Make sure the request came from Bitrix24:

- compare `auth.application_token` with the token retained for this `member_id`
- if there is no retained token or it does not match, check the access token with the [app.info](../../../api-reference/common/system/app-info.md) method on the authorization server `https://oauth.bitrix.info/rest/`

Do not trust the Bitrix24 address from the request: it is set by the sender, whereas the authorization server address is known in advance. There is no retained token during the first installation, and it may not match after an update: `application_token` changes when the application is updated. Pass the token in the `auth` parameter. In the response, compare `result.client_id` with the `client_id` of your application, and `result.install.member_id` with the `member_id` from the request. If both match, store the new `application_token` together with `member_id`. Without an access token, there is nothing to check the first request with — reject it.

A fragment of the `app.info` response on the authorization server:

```json
{
  "result": {
    "client_id": "app.000000000000000000000000",
    "install": {
      "installed": true,
      "member_id": "a223c6b3710f85df22e9377d6c4f7553"
    }
  }
}
```

For details on verification, see the articles [{#T}](../../../api-reference/events/safe-event-handlers.md) and [{#T}](../../oauth/simple-way.md).

The example parses the event and stores the tokens. Authenticity is checked by your application's `isRequestTrusted()` function:

```php
$event = $_POST['event'] ?? '';
$auth = $_POST['auth'] ?? [];
$data = $_POST['data'] ?? [];

if ($event !== 'ONAPPINSTALL' && $event !== 'ONAPPUSERREADY') {
    http_response_code(200);
    exit;
}

if (!isRequestTrusted($auth)) {
    http_response_code(403);
    exit;
}

// for ONAPPINSTALL the authorization is in auth, for ONAPPUSERREADY it is in data
$tokens = $event === 'ONAPPUSERREADY' ? $data : $auth;

// the request may contain no tokens — in that case, keep the retained ones
if (!empty($tokens['access_token']) && !empty($tokens['refresh_token'])) {
    saveTokens($auth['member_id'], $event, [ // your storage function
        'access_token' => $tokens['access_token'],
        'refresh_token' => $tokens['refresh_token'],
        'expires_in' => (int)($tokens['expires_in'] ?? 0),
    ]);
}

http_response_code(200);
echo 'OK';
```

Retain the authorization data of the two events separately. The system user tokens from `ONAPPUSERREADY` remain valid longer and do not depend on the employee who installed the application.

## Continue Your Exploration

- [{#T}](./index.md)
- [{#T}](./rest-only-installation-master.md)
- [{#T}](../../../api-reference/common/events/on-app-install.md)
- [{#T}](../../../api-reference/common/events/on-app-user-ready.md)
- [{#T}](../../../api-reference/events/safe-event-handlers.md)
