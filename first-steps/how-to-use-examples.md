# How to Use Examples in Documentation

The documentation includes code examples for various programming languages in the method descriptions and additional tutorials. Each example is often presented in four formats: a curl request with parameters, JS code using the BX24.js library, PHP code using the CRest SDK, and PHP code using the official B24PhpSdk, as well as JS code using the official B24JsSdk.

## Curl Requests

When using the Bitrix24 REST API via curl, no libraries or SDKs are required. You can create parameters for calling any REST method. You just need to be careful about the correct formation of parameters, especially when it comes to parameters that accept arrays or structures as values.

Example of making a request to the Bitrix24 REST API via curl using a temporary access token [OAuth 2.0](../api-reference/oauth/index.md):

```bash
curl -X POST \
-H "Content-Type: application/json" \
-d '{
"fields": {
"title": "New Deal",
"typeId": "SALE",
"stageId": "NEW"
},
auth=YOUR_ACCESS_TOKEN
}' \
https://your-domain.bitrix24.com/rest/crm.deal.add.json
```

Example of making a request to the Bitrix24 REST API via curl using an [incoming webhook](../local-integrations/local-webhooks.md):

```bash
curl -X POST \
-H "Content-Type: application/json" \
-d '{
"fields": {
"title": "New Deal",
"typeId": "SALE",
"stageId": "NEW"
}
}' \
https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/crm.deal.add.json
```

Replace _USER_ID_ and _CODE_ with actual values from the incoming webhook.

## Connecting Libraries and SDKs

To use the examples, you need to include the corresponding library or SDK in your code. Below are instructions and examples for each option.

### JavaScript Using bx24.js

Examples using the standard [bx24.js library](../api-reference/bx24-js-sdk/index.md) are intended for use within [local](../local-integrations/local-apps.md) or [mass-market applications](../market/index.md). Unfortunately, you cannot simply use it by including the library on an external site or local HTML page.

However, once you understand the concept of a local or even mass-market application, using the JavaScript examples from the documentation will become very straightforward. To use the JavaScript examples, you only need to include the following script:

```html
<script src="//api.bitrix24.com/api/v1/"></script>
```

Example of using bx24.js:

```js
<script src="//api.bitrix24.com/api/v1/"></script>

<script>
BX24.callMethod(
    "crm.deal.add",
    {
        fields: {
        TITLE: "New Deal",
        TYPE_ID: "SALE",
        STAGE_ID: "NEW"
        }
    },
    function(result) {
        if (result.error()) {
            console.error(result.error());
        } else {
            console.log(result.data());
        }
    }
);
</script>
```

Additional information about BX24.js can be found in the section [{#T}](../api-reference/bx24-js-sdk/index.md). Note that this library can only be used within applications that open in frames in the Bitrix24 user interface. Read more about this in the section on [widgets](../api-reference/widgets/index.md).

### JavaScript Using B24JsSDK

Unlike bx24.js, B24JsSDK can be used in any web applications, both as front-end and back-end on Node.js.

This distinction requires a slightly different approach to including the library. Detailed information can be found in [{#T}](../api-reference/b24jssdk/index.md). Here is an example of including B24JsSDK as a browser UMD library:

```html
<script src="https://unpkg.com/@bitrix24/b24jssdk@latest/dist/umd/index.min.js"></script>
```

After including, the library will be available through the global variable `B24Js`. The example below assumes that the code is used within applications that open in frames in the Bitrix24 user interface:

```html
<script src="https://unpkg.com/@bitrix24/b24jssdk@latest/dist/umd/index.min.js"></script>
<script type="module">
try
{
    const $logger = B24Js.LoggerBrowser.build('local-app', true);
    
    const $b24 = await B24Js.initializeB24Frame();
    $b24.setLogger(
        B24Js.LoggerBrowser.build('Core')
    );
    
    $logger.warn('B24Frame.init');
    
    const response = await $b24.callMethod(
        'crm.item.add',
        {
            entityTypeId: B24Js.EnumCrmEntityTypeId.deal,
            fields: {
                title: `New Deal`,
                typeId: 'SALE',
                stageId: 'NEW'
            }
        }
    );
    
    const newDeal = response.getData().result.item;
    
    $logger.info(
        `${B24Js.Text.getDateForLog()} crm.item.add >>`,
        {
            newId: newDeal.id,
            createdTime: B24Js.Text.toDateTime(newDeal.createdTime).toFormat('HH:mm:ss'),
            fields: newDeal
        }
    );
}
catch( error )
{
    console.error(error);
}
</script>
```

The library also supports working with incoming webhooks. However, in this case, it should be used on the back-end in Node.js. Calling the REST API using an incoming webhook from the front-end is unsafe, as any user who opens such an application can see the webhook and use it for unauthorized access to Bitrix24 with the rights of the user who created the webhook.

### PHP Using B24PhpSDK

To start using it, you need to install and include B24PhpSDK. Detailed information can be found in [{#T}](../api-reference/b24phpsdk/index.md).

Example of using B24PhpSDK with an incoming webhook:

```php

declare(strict_types=1);

// Include the base SDK class
use Bitrix24\SDK\Services\ServiceBuilderFactory;

// ensure the path to autoload.php is correct. It may be different if
// you are using your own folder structure 
require_once 'vendor/autoload.php'; 

$B24 = ServiceBuilderFactory::createServiceBuilderFromWebhook(
    '--insert your webhook code here--'
);

$result = $B24->getCRMScope()->deal()->add([
    'TITLE' => 'New Deal',
    'TYPE_ID' => 'SALE',
    'STAGE_ID' => 'NEW'
])->getId();
```

### PHP Using CRest SDK

To use the PHP examples, you need to install and include the CRest SDK. Detailed information can be found in [{#T}](../api-reference/crest-php-sdk/index.md).

Example of using CRest SDK:

```php
require_once('crest.php'); // include CRest PHP SDK

// make a request to the REST API
$result = CRest::call(
    'crm.deal.add',
    [
        'fields' => [
            'TITLE' => 'New Deal',
            'TYPE_ID' => 'SALE',
            'STAGE_ID' => 'NEW'
        ]
    ]
);

// Process the response from Bitrix24
if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
}
```