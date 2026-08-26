# B24PhpSDK: Installation and First Call

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

B24PhpSDK is the official PHP library for server-side applications and integrations with Bitrix24. Without an SDK, a developer has to send HTTP requests to the methods manually, pass authorization data, and parse JSON responses. B24PhpSDK wraps these actions into PHP classes and methods.

To let the SDK reach Bitrix24, select a connection scenario: an incoming webhook, a local application, or a mass-market application. The table below helps you choose the right option.

| If You Need to | Select the Option |
|---|---|
| Set up an internal integration without creating an application | [Incoming Webhook](#incoming-webhook) |
| Create an application for a single Bitrix24 | [Local Application](#local-app) |
| Install the application on different Bitrix24 accounts | [Mass-Market Application](#market-app) |

## Getting Started

Create or open the PHP project that will run the integration with Bitrix24. You need to add the B24PhpSDK library to this project, then select a way to connect to Bitrix24 and make the first method call.

Before you start, make sure you have:

- a PHP project on a local machine or a server. An empty directory where `composer.json` will be created also works
- [Composer](https://getcomposer.org/) to install dependencies
- PHP 8.2–8.4 for SDK v1 or PHP 8.4+ for SDK v3, with the `curl`, `intl`, and `json` extensions
- a Bitrix24 where you can create a webhook or install an application
- access to CRM — it is required for the deal creation example

### Add B24PhpSDK to a PHP Project

Open the directory of your PHP project in the terminal. This is usually the directory that contains `composer.json` or where it is to be created.

Add the `bitrix24/b24phpsdk` Composer package. Composer downloads B24PhpSDK and its dependencies into the `vendor` directory and prepares class autoloading.

For current requirements, branches, and examples, see the [official B24PhpSDK repository](https://github.com/bitrix24/b24phpsdk).

Select the major version of the SDK for your project:

- v1 — the stable option for production and PHP 8.2–8.4
- v3 — the option for PHP 8.4+ and new API methods. v3 contains breaking changes

If you run `composer require bitrix24/b24phpsdk` without a version constraint, Composer selects the latest compatible version of the package. On PHP 8.4, this may be v3. To pin the major version, specify it explicitly:

```bash
# stable v1 for PHP 8.2–8.4
composer require bitrix24/b24phpsdk:"^1.0"

# v3 for PHP 8.4+ and new API methods
composer require bitrix24/b24phpsdk:"^3.3"
```

For a web application, it is convenient to keep public files separate from the code and dependencies:

```text
/vendor              # Composer dependencies, including B24PhpSDK
/public              # files available to the web server
    index.php
    install.php
/src                 # application business logic
/var/log             # application logs
/composer.json
/composer.lock
```

The web server must expose only the `public` directory or its equivalent. `vendor`, `src`, `var`, `composer.json`, and `composer.lock` must not be accessible from the browser.

### Connect the SDK Through an Incoming Webhook {#incoming-webhook}

An incoming webhook is a URL with an access key that you can use to call Bitrix24 methods. A webhook is suitable for an internal integration without an application.

Create an [incoming webhook](../../local-integrations/local-webhooks.md) in Bitrix24 and select its permissions. The deal example requires CRM. Copy the full webhook URL and pass it to `ServiceBuilderFactory::createServiceBuilderFromWebhook`:

```php
<?php

declare(strict_types=1);

use Bitrix24\SDK\Services\ServiceBuilderFactory;

require_once __DIR__ . '/../vendor/autoload.php';

$b24Service = ServiceBuilderFactory::createServiceBuilderFromWebhook(
    'https://example.bitrix24.com/rest/1/your-webhook-code/'
);
```

After initialization, call the method through a ready-made SDK service. For a deal, use `entityTypeId = 2` — this is the identifier of the "Deal" CRM object type. The code below creates a deal and writes its identifier to `$dealId`:

```php
$result = $b24Service->getCRMScope()->item()->add(
    2,
    [
        'title' => 'New Deal',
    ]
);

$dealId = $result->item()->id;

echo 'Created deal ID: ' . $dealId;
```

If the request succeeds, the script prints the identifier of the new deal, and the "New Deal" deal appears in Bitrix24 CRM. If an error is returned instead of the identifier, check the webhook URL and its permissions.

### Connect the SDK in a Local Application {#local-app}

A local application works within a single Bitrix24 and uses OAuth 2.0. When the application is opened, Bitrix24 passes the authorization parameters in the HTTP request. The SDK takes them from the `Request` object and creates a service for calling methods.

Select an [installation scenario for a local application](../../settings/app-installation/local-apps/index.md) and fill in the application card. Copy `client_id`, `client_secret`, and the scope from the application settings into the `ApplicationProfile::initFromArray` array:

```php
<?php

declare(strict_types=1);

use Bitrix24\SDK\Core\Credentials\ApplicationProfile;
use Bitrix24\SDK\Services\ServiceBuilderFactory;
use Symfony\Component\HttpFoundation\Request;

require_once __DIR__ . '/../vendor/autoload.php';

$appProfile = ApplicationProfile::initFromArray([
    'BITRIX24_PHP_SDK_APPLICATION_CLIENT_ID' => 'put-your-client-id-here',
    'BITRIX24_PHP_SDK_APPLICATION_CLIENT_SECRET' => 'put-your-client-secret-here',
    'BITRIX24_PHP_SDK_APPLICATION_SCOPE' => 'crm,user_basic',
]);

$b24Service = ServiceBuilderFactory::createServiceBuilderFromPlacementRequest(
    Request::createFromGlobals(),
    $appProfile
);
```

`createServiceBuilderFromPlacementRequest` expects the application to be opened from Bitrix24, with `DOMAIN` present in the GET parameters of the request. If you open the file directly without the application opening parameters, the SDK cannot determine the Bitrix24 address. For external applications that are not opened from the Bitrix24 interface, use other ways to initialize `ServiceBuilder` — see the examples in the [official B24PhpSDK repository](https://github.com/bitrix24/b24phpsdk).

After initialization, use the same services as for the webhook:

```php
$result = $b24Service->getCRMScope()->item()->add(
    2,
    [
        'title' => 'New Deal',
    ]
);

$dealId = $result->item()->id;
```

### Prepare a Mass-Market Application {#market-app}

A mass-market application is installed on different Bitrix24 accounts. It also uses OAuth 2.0, but the application needs its own storage for tokens and installation data.

Select an [installation scenario for a mass-market application](../../settings/app-installation/mass-market-apps/index.md). In the code, you can use `ApplicationProfile` and `ServiceBuilderFactory`, but token storage, token renewal, and handling of different Bitrix24 accounts have to be designed separately.

For the implementation, use the SDK materials:

- [an example application with OAuth token storage](https://github.com/bitrix24/b24phpsdk/tree/v3/tests/ApplicationBridge)
- [the `Bitrix24\SDK\Application\Contracts` contracts](https://github.com/bitrix24/b24phpsdk/tree/v3/src/Application/Contracts)
- [an example of subscribing to `AuthTokenRenewedEvent`](https://github.com/bitrix24/b24phpsdk/blob/v3/tests/Integration/Factory.php)

### Make a Universal Method Call

If there is no ready-made SDK service for the method you need, call the method through `core->call`. In this case, the parameters have to be passed in the structure of the REST method. For example, `crm.item.add` requires the identifier of the CRM object type and the fields of the new item:

```php
$response = $b24Service->core->call('crm.item.add', [
    'entityTypeId' => 2,
    'fields' => [
        'title' => 'New Deal',
    ],
]);

$dealId = $response
    ->getResponseData()
    ->getResult()['item']['id'];

echo 'Created deal ID: ' . $dealId;
```

A universal call is convenient for methods that do not yet have a dedicated wrapper in the SDK. However, the IDE does not suggest the method signature as precisely as it does for a call through a ready-made service.

## Videos with Examples

The text scenarios above show the minimal path to the first call. You can use the videos as an additional walkthrough after the setup.

### Incoming Webhook

@[youtube](https://youtu.be/H5rBky_DJ4c?si=YPzS64M0JaVDABIJ)

[Download the incoming webhook example](https://helpdesk.bitrix24.com/examples/b24phpsdk-webhook-example.zip)

### Local Application

@[youtube](https://youtu.be/bgbzmq63EsM?si=zpfCrZhmfaJqfDhA)

[Download the local application example](https://helpdesk.bitrix24.com/examples/b24phpsdk-local-app-example.zip)

### Local Application with Token Storage

@[youtube](https://youtu.be/eE-YqwxmzBk?si=3seaxKPX70N_jokI)

## Integration With Other Tools

**Other SDKs.** To compare B24PhpSDK with other libraries, see the [SDK Overview](../index.md). If you need a minimal starter set of PHP files without typed services, see [CRest PHP SDK](../crest-php-sdk/index.md).

**Authorization.** For an internal integration, use an [incoming webhook](../../local-integrations/local-webhooks.md). For applications with OAuth 2.0, see the [application installation scenarios](../../settings/app-installation/index.md) and the [OAuth description](../../settings/oauth/index.md).

**Permissions.** The scope depends on the methods that the application or the webhook calls. See scope values in the [Permissions](../../api-reference/scopes/permissions.md) guide.

## Key Considerations

- Do not publish the incoming webhook URL, `client_secret`, the access token, or the refresh token
- Data access depends on the permissions of the user on whose behalf the request is performed, and on the scope of the webhook or the application
- B24PhpSDK adds the `X-Request-ID` request identifier and, for most methods, the `bx24_request_id` parameter to simplify request diagnostics
- When the access token expires, the SDK can renew the token and raise the `AuthTokenRenewedEvent` event; the application has to retain the new token in its own storage
- For bulk operations, use the batch services of the SDK: they return PHP generators and help process large data sets without loading all results into memory
- The typed services of the SDK convert part of the REST API data into PHP types, for example date and time, so use a ready-made service first and reserve `core->call` for methods without a wrapper
