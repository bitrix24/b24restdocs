# Installation Callback

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

An application without an interface still needs authorization tokens. The installation callback is the way to obtain them: Bitrix24 sends the tokens to your handler right after a user installs a mass-market application.

## When to Use It

The option suits applications that do not have their own page in Bitrix24. To achieve this, the "Add your page and item to the main menu" option is left unchecked in the application card. No item appears in the left menu, but the application can still register widgets in embedding locations. The entire business logic of such an application runs in event handlers.

The handler URL is specified in the application card, in the "Event installation handler URL" field. Bitrix24 accepts an HTTP or HTTPS URL, but tokens are sent in the request, so use HTTPS.

If one-time settings are required during installation, the callback is not a good fit. Choose the [Installation Wizard](./installation-master.md) for an application with an interface, or the [Configuration Wizard for REST-Only Applications](./rest-only-installation-master.md). All four options are compared in the overview [{#T}](./index.md).

## How It Works

Bitrix24 registers the specified address as an event handler. That is why Bitrix24 sends a standard event request to this URL: the `event`, `data`, and `ts` fields and the `auth` object with OAuth 2.0 authorization data — the access token, the refresh token, and their lifetime.

Two different events arrive at the same address.

#|
|| **Event** | **When It Arrives** | **What the Application Receives** ||
|| [`ONAPPUSERREADY`](../../../api-reference/common/events/on-app-user-ready.md) | When Bitrix24 has created or re-activated the application system user | Long-lived authorization of the application system user ||
|| [`ONAPPINSTALL`](../../../api-reference/common/events/on-app-install.md) | Additionally, if the application works only through the API, without its own page in Bitrix24 | Authorization of the employee who installed the application ||
|#

This is why the handler branches on the `event` field. Without it, the application will treat the second event as a repeated installation.

The authorization data of the events is located in different parts of the request. For `ONAPPINSTALL`, it is sent in the `auth` object. For `ONAPPUSERREADY`, the authorization of the system user arrives in `data`, while `auth` holds the authorization of the employee who installed the application. The `application_token` used to verify authenticity is present in `auth` for both events.

An example of the request, with an abbreviated `auth` object:

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

Each event has its own full set of fields, described on the pages of these events.

## What to Return

The installation does not depend on the handler response: for an application without an interface, it is considered complete as soon as the application is added. Return code 200 and retain the tokens on your side: the access token has a limited lifetime and is renewed with the refresh token.

The handler is available at a public address, so anyone can send a request to it. Make sure the request came from Bitrix24: the `auth` object of both events carries `application_token`, which is constant for the application on a specific Bitrix24 account.

During the first installation, there is nothing to compare `application_token` with yet, so a working access token confirms the authenticity of the request: check it and only then retain `application_token`. On subsequent calls, compare the received token with the retained one. For details, see the article [{#T}](../../../api-reference/events/safe-event-handlers.md).

```php
$event = $_POST['event'] ?? '';
$auth = $_POST['auth'] ?? [];
$data = $_POST['data'] ?? [];

if ($event !== 'ONAPPINSTALL' && $event !== 'ONAPPUSERREADY') {
    header('HTTP/1.1 200 OK');
    exit;
}

$applicationToken = $auth['application_token'] ?? '';
$memberId = $auth['member_id'] ?? '';

// your storage functions, the key is member_id: one handler accepts events from different Bitrix24 accounts
$savedToken = loadApplicationToken($memberId);

if ($savedToken === '') {
    if (!isAccessTokenValid($auth['client_endpoint'] ?? '', $auth['access_token'] ?? '')) {
        header('HTTP/1.1 403 Forbidden');
        exit;
    }

    saveApplicationToken($memberId, $applicationToken);
} elseif (!hash_equals($savedToken, $applicationToken)) {
    header('HTTP/1.1 403 Forbidden');
    exit;
}

// for ONAPPINSTALL the authorization is in auth, for ONAPPUSERREADY it is in data
$tokens = $event === 'ONAPPUSERREADY' ? $data : $auth;

saveTokens($memberId, $event, [ // your storage function
    'access_token' => $tokens['access_token'] ?? '',
    'refresh_token' => $tokens['refresh_token'] ?? '',
    'expires_in' => (int)($tokens['expires_in'] ?? 0),
]);

header('HTTP/1.1 200 OK');
echo 'OK';
```

A working access token is checked by calling the [profile](../../../api-reference/common/users/profile.md) method at the address from `client_endpoint`: the method requires no scopes, and it returns a result only for a token issued by this Bitrix24. Send the token in the body of the POST request rather than in the URL, so that it does not end up in the web server logs.

```php
function isAccessTokenValid(string $clientEndpoint, string $accessToken): bool
{
    if ($clientEndpoint === '' || $accessToken === '') {
        return false;
    }

    $ch = curl_init($clientEndpoint . 'profile');
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_TIMEOUT, 10);
    curl_setopt($ch, CURLOPT_POST, true);
    curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query(['auth' => $accessToken]));
    $response = curl_exec($ch);
    curl_close($ch);

    if ($response === false) {
        return false;
    }

    $result = json_decode($response, true);

    return isset($result['result']);
}
```

Retain the authorization data of the two events separately: from `ONAPPINSTALL` you receive the tokens of the employee who installed the application, and from `ONAPPUSERREADY` the tokens of the application system user, which remain valid longer and do not depend on that employee.

The [BX24.installFinish()](../../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md) method does not need to be called in this scenario, unlike in the [Installation Wizard](./installation-master.md). This is a method of the JS library: it works only within the frame of the application interface, while the handler is invoked from the Bitrix24 server, and no browser is involved in the process.

## Continue Your Exploration

- [{#T}](./index.md)
- [{#T}](./rest-only-installation-master.md)
- [{#T}](../../../api-reference/common/events/on-app-install.md)
- [{#T}](../../../api-reference/common/events/on-app-user-ready.md)
- [{#T}](../../../api-reference/events/safe-event-handlers.md)
