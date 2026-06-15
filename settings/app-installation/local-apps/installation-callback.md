# Installation callback

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

When a local application is added with the **Uses API only** option, it does not have a base interface in the left menu. Such an application can still register widgets in various integration points, but a menu item will not be automatically added to the left menu.

This is a useful scenario for applications that do not require a separate user interface or settings, and where all business logic is implemented, for example, as automatic event handlers.

Despite the lack of a user interface, such applications need to retrieve authorization tokens. For this, a callback handler is required, the path to which must be specified in the **Initial installation path** field.

Immediately after adding a local application, Bitrix24 will automatically call this URL and send a POST request with OAuth 2.0 authorization data, including `access_token` and `refresh_token`.

The application must retain these tokens on its side. `access_token` is valid for a limited time, so permanent access to the REST API is provided not by a perpetual token, but by a retained `refresh_token`.

## Example of retrieving and retaining tokens

The callback handler must accept the installation POST request and retain the `auth` object. For the further operation of the application, it is important `refresh_token`: the application can use it to retrieve a new token pair without user involvement.

```php
<?php
const AUTH_FILE = __DIR__ . '/auth.json';

function saveAuth(array $auth): void
{
    file_put_contents(AUTH_FILE, json_encode($auth, JSON_UNESCAPED_SLASHES));
}

if (isset($_POST['auth']) && is_array($_POST['auth']))
{
    saveAuth($_POST['auth']);

    header('Content-Type: application/json');
    echo json_encode(['status' => 'success']);
    exit();
}

http_response_code(400);
echo 'Auth data is required';
```

The retained `refresh_token` must be used for [automatic OAuth 2.0 token renewal](../../oauth/auto-renewal.md). During each renewal, Bitrix24 returns a new pair of `access_token` and `refresh_token`, which must be overwritten on the application side.

In the example, tokens are written to the `auth.json` file. In a production application, retain tokens in a data store that is inaccessible from the browser.

## Obtaining the first token pair via full OAuth

If the application did not retain `refresh_token` during installation or if you need to re-obtain the first token pair, use the [full OAuth 2.0 authorization protocol](../../oauth/index.md). This scenario does not replace the installation callback, but rather helps re-run the application's primary authorization.

Take `client_id` and `client_secret` from the local application form. The redirect URI that Bitrix24 uses to return the user after authorization is taken from the application settings.

Form an authorization link and open it in a browser as the user who is granting the application access to Bitrix24.

```php
<?php
const CLIENT_ID = 'local.000000000000000000000000';

$domain = 'example.bitrix24.com';

$authorizeUrl = 'https://' . $domain . '/oauth/authorize/?' . http_build_query([
    'client_id' => CLIENT_ID,
]);

header('Location: ' . $authorizeUrl);
exit();
```

After authorization, Bitrix24 will redirect the user to `redirect_uri` and pass the `code` parameter. This is not a REST API token, but a one-time authorization code. The lifetime of `code` is 30 seconds, so it must be exchanged for tokens immediately.

```php
<?php
const CLIENT_ID = 'local.000000000000000000000000';
const CLIENT_SECRET = 'client_secret';
const AUTH_FILE = __DIR__ . '/auth.json';

if (!isset($_GET['code']))
{
    http_response_code(400);
    echo 'Authorization code is required';
    exit();
}

$query = http_build_query([
    'grant_type' => 'authorization_code',
    'client_id' => CLIENT_ID,
    'client_secret' => CLIENT_SECRET,
    'code' => $_GET['code'],
]);

$response = file_get_contents('https://oauth.bitrix.info/oauth/token/?' . $query);
$auth = json_decode($response, true);

if (!is_array($auth) || isset($auth['error']))
{
    http_response_code(400);
    echo $response;
    exit();
}

file_put_contents(AUTH_FILE, json_encode($auth, JSON_UNESCAPED_SLASHES));

echo 'Authorization tokens saved';
```

This scenario does not create a permanent `access_token`. The application receives the first pair of tokens and saves them `refresh_token` and uses them to extend access. If `refresh_token` is not updated, it will expire.

In the case of an installation callback handler, the application **must not** call the JS method [BX24.installFinish()](../../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md), as is required for local applications using an [installation wizard](./installation-master.md).

Even if an attempt is made to do so, the method will not work. `BX24.installFinish()` only works within the application interface frames in a browser, whereas the application callback handler is called from the Bitrix24 server side. The browser is not involved in this process.