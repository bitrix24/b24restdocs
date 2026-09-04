# Working in the Context of the Current User

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

By default, CRest executes requests under the user who installed the application. To have a request executed under the user who opened the application in a frame, override the token source in the `CRest` class.

The tokens arrive from Bitrix24 every time the application is opened in a frame: `AUTH_ID`, `REFRESH_ID`, `DOMAIN`, and `APP_SID` are sent in a POST request to the handler address. The full set of data is covered in the article [Simplified Method for Obtaining OAuth 2.0 Tokens](../../settings/oauth/simple-way.md), and the composition of the POST request for embedding locations is covered in the [Widget Overview](../../api-reference/widgets/index.md).

## What You Need Before You Start

- the application is installed, and `settings.json` is placed next to `crest.php`. The class substitutes only the user tokens, while `client_endpoint`, `client_id`, and `client_secret` are still taken by the library from the settings file
- the handler verifies that the request came from Bitrix24. The handler address is reachable from the external network, so compare `APPLICATION_TOKEN` with the value the application retained during installation — how this is done is described in the [Widget Overview](../../api-reference/widgets/index.md)
- the `AUTH_ID` and `REFRESH_ID` values do not end up in logs and are not passed to third parties

## How the Override Works

CRest calls `getSettingData` before every request to retrieve the application settings. The base implementation reads the whole `settings.json`. The subclass preserves the remaining values from the file and substitutes four of them: `access_token`, `domain`, `refresh_token`, and `application_token`.

The source of these values depends on whether `setDataExt` was called. If it was, the tokens are taken from the array that was passed; if it was not, they are taken directly from `$_REQUEST`.

{% note warning "" %}

`APP_SID` and `APPLICATION_TOKEN` are different values, and one must not be substituted for the other. `APP_SID` is a session identifier, and Bitrix24 creates it anew every time the application is rendered. `APPLICATION_TOKEN` is a permanent application token that the handler uses to verify that the request came from Bitrix24.

The class puts `APP_SID` into the `application_token` setting — CRest itself does the same during installation. The library neither sends this setting anywhere nor compares it with anything: it only needs it to consider the settings filled in. To verify the request source, take `APPLICATION_TOKEN` from the request rather than the CRest setting.

{% endnote %}

```php
require_once(__DIR__ . '/crest.php');
class CRestCurrent extends CRest
{
    protected static $dataExt = [];
    protected static function getSettingData()
    {
        $return = static::expandData(file_get_contents(__DIR__ . '/settings.json'));
        if(is_array($return))
        {
            if(!empty(static::$dataExt))
            {
                $return['access_token'] = htmlspecialchars(static::$dataExt['AUTH_ID']);
                $return['domain'] = htmlspecialchars(static::$dataExt['DOMAIN']);
                $return['refresh_token'] = htmlspecialchars(static::$dataExt['REFRESH_ID']);
                $return['application_token'] = htmlspecialchars(static::$dataExt['APP_SID']);
            }
            else
            {
                $return['access_token'] = htmlspecialchars($_REQUEST['AUTH_ID']);
                $return['domain'] = htmlspecialchars($_REQUEST['DOMAIN']);
                $return['refresh_token'] = htmlspecialchars($_REQUEST['REFRESH_ID']);
                $return['application_token'] = htmlspecialchars($_REQUEST['APP_SID']);
            }
        }
        return $return;
    }
    public static function setDataExt($data)
    {
        static::$dataExt = $data;
    }
}
```

Save the class in a separate file next to `crest.php` — `crestcurrent.php`, for example — and include it on the application pages.

## How to Pass the Tokens Explicitly

Bitrix24 passes the tokens only when the application is opened in a frame. Subsequent requests from the page — AJAX, for example — arrive without them.

The handler works as follows:

1. Bitrix24 opens the application page with a POST request carrying the tokens.
2. The handler compares `APPLICATION_TOKEN` with the value retained during installation.
3. The handler retains the tokens — in the session, for example.
4. The page includes `crestcurrent.php`, passes the tokens with the `setDataExt` method, and calls methods through `CRestCurrent`.

On subsequent requests, the first three steps are not performed, and `setDataExt` substitutes the retained tokens:

```php
session_start();
require_once(__DIR__ . '/crestcurrent.php');

// The tokens arrive only in the request from Bitrix24
if (isset($_REQUEST['AUTH_ID'])) {

    // $savedApplicationToken is the APPLICATION_TOKEN retained by the application during installation
    if (!hash_equals($savedApplicationToken, $_REQUEST['APPLICATION_TOKEN'] ?? '')) {
        http_response_code(403);
        exit;
    }

    $_SESSION['b24_tokens'] = [
        'AUTH_ID'    => $_REQUEST['AUTH_ID'],
        'REFRESH_ID' => $_REQUEST['REFRESH_ID'],
        'DOMAIN'     => $_REQUEST['DOMAIN'],
        'APP_SID'    => $_REQUEST['APP_SID'],
    ];
}

CRestCurrent::setDataExt($_SESSION['b24_tokens'] ?? []);
```

The array must contain all four keys: if at least one of them is empty, CRest considers the settings incomplete.

The tokens are issued for the user who opened the application, so the calls are limited by that user's permissions in Bitrix24 and by the application scopes. The list of the granted scopes arrives in the same request in the `APPLICATION_SCOPE` parameter.

## What Happens When the Token Expires

`AUTH_ID` is valid for one hour. When it expires, CRest retrieves a new pair of tokens with the `refresh_token` and retains it with the `setSettingData` method — that is, it writes it to `settings.json`.

The class above overrides only the reading of the settings. The base implementation still handles writes, so the tokens of the user who opened the application will be written to the file over the ones retained during installation. If several people use the application, override `setSettingData` as well to retain the tokens separately for each user. Retaining tokens on your own side is covered in the "Key Considerations" section of the [CRest PHP SDK](./index.md) overview.

## Verification

```php
$result = CRestCurrent::call('user.current');

echo '<pre>';
    print_r($result);
echo '</pre>';
```

The method returns the data of the user who opened the application, not of the one who installed it.

If a `no_install_app` error is returned instead of the result, the cause is not necessarily the installation. CRest also returns it when at least one of the `access_token`, `domain`, `refresh_token`, `application_token`, `client_endpoint` values is empty in the settings. The most common cause is that the page was opened directly by its address, without a POST request from Bitrix24, and there was nothing to substitute into the settings.

## Continue Your Exploration

- [{#T}](./index.md)
- [{#T}](../../settings/oauth/simple-way.md)
- [{#T}](../../settings/oauth/auto-renewal.md)
- [{#T}](../../api-reference/widgets/index.md)
- [{#T}](../../local-integrations/serverside-local-app-with-ui.md)
