# Installation callback

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The installation callback is a way to receive authorization tokens on your server. Right after a local application is installed, Bitrix24 sends an [ONAPPINSTALL](../../../api-reference/common/events/on-app-install.md) event in a POST request to the address from the *Initial installation path* field. The OAuth 2.0 authorization data, including `access_token` and `refresh_token`, arrives in the `auth` object.

## When to Use

This option suits applications that do not need initial configuration: for example, when all the logic runs in event handlers. If you do not need an interface, leave the *Menu item text* fields empty — the application will not appear in the left menu but will be able to register widgets in integration points.

Leave the *Application completes the installation itself* option disabled, as it is by default. If you enable it, Bitrix24 does not send the data to the handler and waits for a [BX24.installFinish()](../../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md) call on the [installation wizard](./installation-master.md) page.

## How to Configure the Installation

The handler from the example checks the token from the installation request and stores the authorization data in a file. For the handler to receive the request:

1. Create a local application: fill in the *Your handler path* field and select the access permissions. Leave the *Initial installation path* field empty for now. After you save the form, the application's `client_id` appears in it
2. Specify the `client_id` in the handler code and place the handler on your server accessible over HTTPS. The example below requires PHP 7.4 or later with the cURL extension and a `/var/app-data` directory that PHP can write to
3. Enter the handler address in the *Initial installation path* field and save the form
4. Click *Reinstall* in the form — only an administrator can do this. Bitrix24 sends the installation request to the handler

## Handler Example

A request fragment, values shortened:

```php
$_POST = [
  'event' => 'ONAPPINSTALL',
  'auth' => [
    'access_token' => '***',
    'refresh_token' => '***',
    'expires_in' => 3600,
    'client_endpoint' => 'https://example.bitrix24.com/rest/',
    'member_id' => '***',
    'application_token' => '***'
  ]
];
```

The handler is available at a public address, so anyone can send a request to it. Before storing the tokens, check them with the [app.info](../../../api-reference/common/system/app-info.md) method.

A local application works in one Bitrix24, and its address is known in advance. The `client_endpoint` address in the request is set by the sender, so call the method with the received `access_token` at the address of your own Bitrix24. If the token was issued to your application, the `CODE` field in the response matches the application's `client_id`. A response fragment:

```json
{
  "result": {
    "ID": 1,
    "CODE": "local.000000000000000000000000",
    "STATUS": "L",
    "INSTALLED": true
  }
}
```

```php
<?php
const CLIENT_ID = 'local.000000000000000000000000';
// REST API address of your Bitrix24
const PORTAL_ENDPOINT = 'https://example.bitrix24.com/rest/';
// store the tokens outside the public website directory
const AUTH_FILE = '/var/app-data/auth.json';

$auth = $_POST['auth'] ?? [];

$ch = curl_init(PORTAL_ENDPOINT . 'app.info');
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query(['auth' => $auth['access_token'] ?? '']));
$response = curl_exec($ch);
if ($response === false)
{
    error_log('app.info: ' . curl_error($ch));
}
curl_close($ch);

$result = json_decode((string)$response, true);
if (isset($result['error']))
{
    error_log('app.info: ' . $result['error']); // do not write tokens to the log
}

$required = ['access_token', 'refresh_token', 'member_id', 'application_token'];
$isComplete = count(array_filter($required, fn($key) => !empty($auth[$key]))) === count($required);

if (($_POST['event'] ?? '') !== 'ONAPPINSTALL' || !$isComplete || ($result['result']['CODE'] ?? '') !== CLIENT_ID)
{
    http_response_code(403);
    exit();
}

if (file_put_contents(AUTH_FILE, json_encode($auth, JSON_UNESCAPED_SLASHES)) === false)
{
    http_response_code(500);
    exit();
}
```

After reinstalling, check that the `/var/app-data/auth.json` file has appeared and contains `access_token`, `refresh_token`, `member_id`, and `application_token`. If the file is missing, look at the handler response and the PHP error log:

- the log contains `app.info:` and a cURL error message — the application server could not reach Bitrix24. Check the address in `PORTAL_ENDPOINT` and the server's access to it, then click *Reinstall*
- the log contains `app.info:` and an error code — the method returned an error. For example, `invalid_token` means that Bitrix24 did not accept the token, and `expired_token` means that the token has expired. Other codes are described in the article [{#T}](../../../error-codes.md). If the request was genuine, click *Reinstall*
- a `403` response with no log entry — the request was not an `ONAPPINSTALL` event, `auth` lacks fields, `app.info` returned a non-JSON response, or `CODE` did not match `CLIENT_ID`. If the request was genuine, check the `client_id` in the handler code and the address in `PORTAL_ENDPOINT`, then click *Reinstall*
- a `500` response — PHP could not write the file. Check the directory permissions, then click *Reinstall*

Store `application_token` and `member_id` together with the tokens. Other application handlers use them to verify incoming events; for details, see the article [{#T}](../../../api-reference/events/safe-event-handlers.md).

With the retained `refresh_token`, the application extends access without user involvement. Each renewal returns a new token pair — store it in place of the previous one. The renewal procedure is described in the article [{#T}](../../oauth/auto-renewal.md).

## If the Tokens Were Not Stored

If the application did not store `refresh_token` during installation, retrieve the first token pair via the [full OAuth 2.0 authorization protocol](../../oauth/index.md). Take `client_id` and `client_secret` from the application form. After authorization, Bitrix24 returns the user to the address from the *Your handler path* field. The OAuth response does not contain `application_token`: if it was stored during installation, do not overwrite it. If it was not stored, click *Reinstall* in the application form — the handler receives it again.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./installation-master.md)
- [{#T}](../../oauth/auto-renewal.md)
- [{#T}](../../../api-reference/events/safe-event-handlers.md)
