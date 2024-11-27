# Installation and Usage of B24JsSDK

[B24JsSDK](https://github.com/bitrix24/b24jssdk) is the official library for working with the Bitrix24 REST API in JavaScript. Unlike [BX24.JS](../bx24-js-sdk/index.md), B24JsSDK offers the following advantages:

1. Works both on the frontend in the browser and on the backend in Node.js;
2. Supports authorization via tokens and webhooks;
3. Formats REST API data into corresponding JavaScript types (numbers, IBAN accounts, dates, etc.);
4. Utilizes modern language features - async/await, promises, AsyncGenerator;
5. Automatically generates unique request identifiers, simplifying debugging;
6. And much more!

## Installation

There are several ways to install the SDK:

- For generating and server-side rendering applications on Node.js and Nuxt;
- For using the UMD version of the SDK in the browser via CDN or from a local server.

### Installation in Node.js / Nuxt

Details can be found in the [B24JsSDK documentation](https://bitrix24.github.io/b24jssdk/guide/getting-started.html).

### Usage in the Browser

Include the library via CDN by adding the following tag to your HTML file:

```html
<script src="https://unpkg.com/@bitrix24/b24jssdk@latest/dist/umd/index.min.js"></script>
```

Or download the UMD version from [unpkg.com](https://unpkg.com/@bitrix24/b24jssdk@latest/dist/umd/index.min.js) and add it to your project:

```html
<script src="/path/to/umd/index.min.js"></script>
```

After including, the library will be available through the global variable `B24Js`. Here’s an example of initialization:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bitrix24 Frame Demo</title>
</head>
<body>
<p>See the result in the developer console</p>
<script src="https://unpkg.com/@bitrix24/b24jssdk@latest/dist/umd/index.min.js"></script>
<script>
    document.addEventListener('DOMContentLoaded', async () => {
        try
        {
            let $b24 = await B24Js.initializeB24Frame();
        }
        catch (error)
        {
            console.error(error);
        }
    });
</script>
</body>
</html>
```

## Usage with Webhooks

For working with local webhooks, use them only in [server applications](https://bitrix24.github.io/b24jssdk/guide/getting-started.html).

Example of server usage of the SDK: [https://github.com/bitrix24/b24sdk-examples/tree/main/js/05-node-hook](https://github.com/bitrix24/b24sdk-examples/tree/main/js/05-node-hook)

## Usage in Frontend Applications

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
                title: `New Deal ${B24Js.Text.getUuidRfc4122()}`,
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
    
    // Open the added deal in the standard slider
    $b24.slider.openPath(
        $b24.slider.getUrl(`/crm/deal/details/${newDeal.id}/`),
        950
    );
}
catch( error )
{
    console.error(error);
}
</script>
```

## Examples

Examples of using the SDK can be found in the repository: [https://github.com/bitrix24/b24sdk-examples/tree/main/js](https://github.com/bitrix24/b24sdk-examples/tree/main/js):

- Creating a user interface in the style of Bitrix24;
- Using webhooks;
- Authorization via tokens;
- Using the UMD version in the browser;
- Using the SDK on the backend in Node.js.