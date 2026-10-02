# Get Interface Language Code BX24.getLang

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.getLang(): string;
```

The `BX24.getLang` method returns the code of the language in which the user sees the Bitrix24 interface, for example `ru` or `en`. The application can use this code to choose the language of its own interface.

The value comes from Bitrix24 during library initialization, so call the method in the [BX24.init](../system-functions/bx24-init.md) handler. The method requires no scope of its own.

{% note info "" %}

Bitrix24 language codes do not always match the ISO 639-1 standard. For example, the code for Ukrainian is `ua`, for Spanish `la`, for Portuguese `br`, and for Chinese `sc` and `tc`.

{% endnote %}

## Method Parameters

No parameters.

## Code Example

{% include [Example Notes](../../../_includes/examples.md) %}

Load the application texts in the user's language, or in English for languages without a translation:

```js
BX24.init(function () {
    const supported = ['ru', 'en', 'de'];
    const lang = supported.includes(BX24.getLang()) ? BX24.getLang() : 'en';

    BX24.loadScript('lang/' + lang + '.js', function () {
        console.log('Texts loaded for language', lang);
    });
});
```

## Response Handling

The method synchronously returns a result of type `string`. Example result:

```json
"ru"
```

### Returned Data

#| 
|| **Name**
`type` | **Description** ||
|| **result**
[`string`](../../../api-reference/data-types.md) | The Bitrix24 interface language code, for example `ru` ||
|#

## Error Handling

The method does not return error codes. If you call it before the library is initialized, it returns an empty string.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](../system-functions/bx24-init.md)
- [{#T}](./bx24-load-script.md)