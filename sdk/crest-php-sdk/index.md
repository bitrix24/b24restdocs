# CRest PHP SDK: Installation and First Call

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

CRest PHP SDK is a set of PHP files for calling Bitrix24 methods from a server-side application. You place the files on your own web server, include `crest.php` in your code, and call methods through `CRest::call`.

To let CRest reach Bitrix24, select a connection scenario: an incoming webhook, a local application, or a mass-market application. The table below helps you choose the right option.

| If You Need to                                            | Select the Option                        |
|-----------------------------------------------------------|------------------------------------------|
| Set up an internal integration without creating an application | [Incoming Webhook](#incoming-webhook)     |
| Create an application for a single Bitrix24               | [Local Application](#local-app)           |
| Install the application on different Bitrix24 accounts    | [Mass-Market Application](#market-app)    |

## Getting Started

First prepare the server, then set up one connection option from the table and make the first call, which is the same for all options.

### Prepare the Server

1. Download the [CRest files](https://github.com/bitrix-tools/crest)
2. Place the contents of the `src` folder on your web server — it contains all the CRest files
3. Make sure the [cURL](https://www.php.net/manual/en/book.curl.php) module is available on the server and a valid SSL certificate is installed
4. Open the `checkserver.php` page in a browser at your site address, for example `https://your-domain.com/checkserver.php` — the script verifies that the cURL module is available and that CRest can retain its files

### Set Up an Incoming Webhook {#incoming-webhook}

An incoming webhook is a link with an access key that you can use to call Bitrix24 methods. A webhook is suitable for an internal integration without an application.

Create an [incoming webhook](../../local-integrations/local-webhooks.md) in Bitrix24 and select its permissions — the example in this article requires CRM. Copy the full webhook URL into the `C_REST_WEB_HOOK_URL` constant in the `settings.php` file:

```php
<?php

define(
    'C_REST_WEB_HOOK_URL',
    'https://example.bitrix24.com/rest/1/your-webhook-code/'
);
```

### Set Up a Local Application {#local-app}

A local application works within a single Bitrix24 and uses the OAuth 2.0 protocol. When the application is installed, Bitrix24 issues it tokens — access keys with a limited lifetime. CRest renews them automatically.

Select an [installation scenario for a local application](../../settings/app-installation/local-apps/index.md) and fill in the application card. If the application has a main page, specify its URL. In the "Path for initial installation" field, specify the URL of the `install.php` file — this file, included with CRest, retrieves the tokens when the application is installed and retains them. Copy `client_id` and `client_secret` from the card into the `settings.php` file:

```php
<?php

define('C_REST_CLIENT_ID', 'your-client-id');
define('C_REST_CLIENT_SECRET', 'your-client-secret');
```

If you entered `client_id` and `client_secret` in `settings.php` after the application was already installed, reinstall it — `install.php` will then run again and retain the tokens.

### Set Up a Mass-Market Application {#market-app}

A mass-market application is intended for installation on different Bitrix24 accounts and also uses OAuth 2.0. Create the application in the partner account and save it to obtain `client_id` and `client_secret`. Specify these values in the `settings.php` file:

```php
<?php

define('C_REST_CLIENT_ID', 'your-client-id');
define('C_REST_CLIENT_SECRET', 'your-client-secret');
```

Select an [installation scenario for a mass-market application](../../settings/app-installation/mass-market-apps/index.md). The scenario you choose determines which field of the version card takes the URL of the `install.php` file:

- the application has an interface — select the scenario with an installation wizard and specify the URL of the `install.php` file in the "Application installer URL" field
- the application has no interface — select the scenario with a callback handler and specify the URL of the `install.php` file as the handler. Bitrix24 calls it automatically during installation

If the application has an interface, specify the URL of its main page in the version card. After saving the version, install the application on a Bitrix24 available to you and check that it works.

### Make the First Call

Once authorization is set up, open the `index.php` file — it is included with CRest and sits next to `crest.php` and `settings.php`. Replace its contents with the code of the first call, or create the file if it does not exist.

The code includes `crest.php` and calls a method through `CRest::call`:

```php
<?php

require_once __DIR__ . '/crest.php';

$result = CRest::call(
    'crm.item.add',
    [
        'entityTypeId' => 3,
        'fields' => [
            'name' => 'Klaus',
            'lastName' => 'Weber',
        ],
    ]
);

if (isset($result['error']))
{
    print_r($result);
}
else
{
    echo 'Contact created with the identifier ' . $result['result']['item']['id'];
}
```

To make the call, open `index.php` in a browser at your site address, for example `https://your-domain.com/index.php`.

The [`crm.item.add`](../../api-reference/crm/universal/crm-item-add.md) method creates a CRM item. The item type is set by the `entityTypeId` parameter: the value `3` is a contact. The method returns the created item in the response, so the identifier of the new contact arrives in `$result['result']['item']['id']`. If an error arrives instead, check the permissions first: the webhook or the application must have access to CRM.

## Integration With Other Tools

**User context.** The authorization method determines on whose behalf CRest performs requests. An incoming webhook performs requests on behalf of the user who created the webhook. Local and mass-market applications use the tokens of the user who installed the application by default. If you need to perform requests on behalf of the user who opened the application, set up [operation in the context of the current user](./using-in-users-context.md).

**Other SDKs.** To compare CRest with other libraries, see the [SDK Overview](../index.md). For PHP, [B24PhpSDK](../b24phpsdk/index.md) is also available.

## Key Considerations

- Do not publish the incoming webhook URL, `client_secret`, or authorization tokens
- Data access depends on the permissions of the user on whose behalf the request is performed. In addition, every method requires its own scope — a permission to work with a specific Bitrix24 section. See scope values in the [Permissions](../../api-reference/scopes/permissions.md) guide
- If the project encoding differs from UTF-8, you may need to change the encoding of the SDK files and declare the `C_REST_CURRENT_ENCODING` constant in `settings.php`, for example `define('C_REST_CURRENT_ENCODING', 'windows-1251');`
- For a mass-market application running on several Bitrix24 accounts, provide your own token storage. To do this, override the `getSettingData` and `setSettingData` methods: the basic implementation retains the tokens of a single Bitrix24 in the `settings.json` file and is not intended for this scenario. An example of inheriting the CRest class with an overridden method is on the page about [operation in the context of the current user](./using-in-users-context.md)
