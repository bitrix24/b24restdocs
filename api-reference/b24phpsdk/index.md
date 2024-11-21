# Loading and Using B24PhpSDK

B24PhpSDK is the official and recommended library for working with the Bitrix24 REST API in PHP. Unlike the CRest SDK, B24PhpSDK offers a number of advantages:

1. Support for code autocompletion and internal structures in VSCode, PHPStorm, PyCharm;
2. Built-in conversion of REST API data to corresponding PHP types (specifically, date/time and money);
3. Efficient use of PHP capabilities - utilizing generators when executing batch requests to save memory, event-driven model for handling special situations like updating authorization tokens, etc.;
4. Auto-generation and "passing through" of unique identifiers for outgoing requests, simplifying debugging in case of lost outgoing events;
5. And many more.

## Installation

To install B24PhpSDK, you can clone the [appropriate repository](https://github.com/bitrix24/b24phpsdk) on GitHub or download the entire SDK as a zip file, but we **strongly recommend** using [composer](https://getcomposer.org/doc/00-intro.md) for installation.

```
composer require bitrix24/b24phpsdk
```

As a result, you will get the correct file structure, which will be convenient to work with later. For example, for a local application, it might look like this:

```
/vendor
    /bitrix24
        /b24phpsdk - the SDK itself
    /yyy - other open-source libraries used in the SDK
/public - folder with the application's "pages"
    event-handler.php – event handler         
    index.php - basic page with the application's UI
    install.php - application installation page
/src
    Application.php - code with the application's business logic         
/var
    /log
        application-YYYY-m-d.log – application logs
/composer.json - composer configuration file, available for modification
/composer.lock - automatically generated composer settings file, used for transferring the assembly of used libraries to the prod environment
```

For working with incoming webhooks, it can be even simpler:

```
/vendor
    /bitrix24
        /b24phpsdk - the SDK itself
    /yyy - other open-source libraries used in the SDK
/public - folder with "functionality"  
    app.php - basic script that calls the REST API        
/var
    /log
        application-YYYY-m-d.log – application logs
/composer.json - composer configuration file, available for modification
/composer.lock - automatically generated composer settings file, used for transferring the assembly of used libraries to the prod environment
```

In both cases, the web server should point to the public folder or its equivalent on your server. Other folders should be inaccessible for security reasons.

## Using with Incoming Webhooks

To connect the SDK within your code (assuming your file is in the public folder from the structure described above), use the minimally required construct:

```php
<?php

declare(strict_types=1);

use Bitrix24\SDK\Services\ServiceBuilderFactory;

// ensure the path to autoload.php is correct. it may differ if
// you are using a different folder structure 
require_once '../vendor/autoload.php'; 

$B24 = ServiceBuilderFactory::createServiceBuilderFromWebhook(
    '--insert your webhook code here--'
);
```

Once the object is initialized, it can be used to call various REST API methods. In the example below, the variable $result will receive the value of the deal identifier as a result of its creation:

```php
$result = $B24->getCRMScope()->deal()->add([
    'TITLE' => 'New Deal',
    'TYPE_ID' => 'SALE',
    'STAGE_ID' => 'NEW'
])->getId();
```

If there is no ready-made "wrapper" for the method you need in the SDK, you can use a universal way to call REST API methods:

```php
$result = $B24->core->call('crm.deal.add', [
    'TITLE' => 'New Deal',
    'TYPE_ID' => 'SALE',
    'STAGE_ID' => 'NEW'
]);
```

In this case, code autocompletion in the IDE does not work, and type casting does not occur in the received data.

[Download example](https://helpdesk.bitrix24.com/examples/b24phpsdk-webhook-example.zip)

## Using in Local and Mass-Market Applications

To connect the SDK within your code (assuming your file is in the public folder from the structure described above), use the minimally required construct:

```php
<?php

declare(strict_types=1);

use Bitrix24\SDK\Core\Credentials\ApplicationProfile;
use Bitrix24\SDK\Services\ServiceBuilderFactory;
use Symfony\Component\HttpFoundation\Request;

require_once 'vendor/autoload.php';

$appProfile = ApplicationProfile::initFromArray([
    'BITRIX24_PHP_SDK_APPLICATION_CLIENT_ID' => 'put-your-client-id-here',
    'BITRIX24_PHP_SDK_APPLICATION_CLIENT_SECRET' => 'put-your-client-secret-here',
    'BITRIX24_PHP_SDK_APPLICATION_SCOPE' => 'crm,user_basic'
]);

$B24 = ServiceBuilderFactory::createServiceBuilderFromPlacementRequest(
    Request::createFromGlobals(), 
    $appProfile
);
```

In the case of a local application, you will need to take the values for `BITRIX24_PHP_SDK_APPLICATION_CLIENT_ID` (Application Code (client_id)), `BITRIX24_PHP_SDK_APPLICATION_CLIENT_SECRET` (Application Key (client_secret)), and `BITRIX24_PHP_SDK_APPLICATION_SCOPE` (Access Permissions) from the corresponding settings of the local application in your Bitrix24.

The code provided above also assumes that the application is opened "inside" Bitrix24 either as the main interface or as a widget. Other scenarios for using the SDK, such as external applications, require different initialization methods.

Once the `$B24` object is initialized, you can use it to call various REST API methods. In the example below, the variable `$result` will receive the value of the deal identifier as a result of its creation:

```php
$result = $B24->getCRMScope()->deal()->add([
    'TITLE' => 'New Deal',
    'TYPE_ID' => 'SALE',
    'STAGE_ID' => 'NEW'
])->getId();
```


[Download example](https://helpdesk.bitrix24.com/examples/b24phpsdk-local-app-example.zip)