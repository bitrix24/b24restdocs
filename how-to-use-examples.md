# How to Use Examples in Documentation

The documentation includes code examples for various programming languages in the method descriptions and additional tutorials. Each example is often presented in four formats: a curl request with parameters, JS code using the BX24.js library, PHP code utilizing the official CRest SDK, and Python code based on the B24-Python SDK.

## Curl Requests

When using the Bitrix24 REST API via curl, no libraries or SDKs are required. You can create parameters for calling any REST method. You just need to be careful about the correctness of the parameters, especially when it comes to parameters that accept arrays or structures as values.

Example of making a request to the Bitrix24 REST API via curl using a temporary access token [OAuth 2.0](./api-reference/oauth/):

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

Example of making a request to the Bitrix24 REST API via curl using an [incoming webhook](./local-integrations/local-webhooks.md):

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

Examples using the standard [bx24.js library](./api-reference/bx24-js-sdk/index.md) are intended for use within [local](./local-integrations/local-apps.md) or [mass-market applications](./market/). Unfortunately, you cannot simply use it by including the library on an external site or a local HTML page.

However, once you understand the concept of local or even mass-market applications, using the JavaScript examples from the documentation will become very straightforward. To use the JavaScript examples, you only need to include the following script:

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

Additional information about BX24.js can be found in the section [{#T}](./api-reference/bx24-js-sdk/index.md). Note that this library can only be used within applications that open in frames in the Bitrix24 user interface. Read more about this in the section on [widgets](./api-reference/widgets/).

### PHP Using B24PhpSDK

To get started, you need to install and include the B24PhpSDK. Detailed information can be found in [{#T}](./api-reference/b24phpsdk/index.md).

Example of using B24PhpSDK with an incoming webhook:

```php

declare(strict_types=1);

// Include the base SDK class
use Bitrix24\SDK\Services\ServiceBuilderFactory;

// ensure the path to autoload.php is correct. It may differ if
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

To use the PHP examples, you need to install and include the CRest SDK. Detailed information can be found in [{#T}](./api-reference/crest-php-sdk/index.md).

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

// Handle the response from Bitrix24
if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
}
```