# Initialize the Library BX24.init

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.init(someCallback: function): void;
```

The `BX24.init` function registers a handler for the "library ready for use" event and executes it as soon as the library receives data from Bitrix24.

During initialization, the library requests the data that the application cannot work without from the parent page:

- the Bitrix24 address and the interface language
- authorization data
- the permissions of the current user
- application configurations
- embedding parameters

Until this data arrives, part of the library's capabilities does not work: [BX24.getAuth](./bx24-get-auth.md) returns `false`, and [BX24.isAdmin](../additional-functions/bx24-is-admin.md) returns `false` regardless of the actual permissions. That is why work with configurations, permissions, and requests to Bitrix24 is performed inside the `BX24.init` handler.

The function works only inside the frame of an [application](../../../settings/app-installation/index.md) and only when the BX24.js library is connected — the [Library Overview](../index.md) describes how to connect it. The function requires no scope of its own.

## Function Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **someCallback***
[`function`](../../../api-reference/data-types.md) | The handler that runs once the library is ready. It is called without parameters, and the library ignores the value it returns ||
|#

## Handler Execution Order

The library executes handlers according to the following rules:

- there can be several handlers: each `BX24.init` call adds its own, and all of them run in the order they were registered
- if the library is already ready, the handler runs asynchronously — the library defers it with a timer instead of calling it immediately
- on the first launch of the application for a user, the handlers wait for the [BX24.install](./bx24-install.md) chain: it ends with a call to [BX24.installFinish](./bx24-install-finish.md)
- [BX24.callMethod](../how-to-call-rest-methods/bx24-call-method.md) calls made before initialization are not lost: the library defers them and executes them once it is ready

Library readiness is not the same as the readiness of the page's DOM structure: [BX24.ready](../additional-functions/bx24-ready.md) and [BX24.isReady](../additional-functions/bx24-is-ready.md) are responsible for that.

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```html
<script src="//api.bitrix24.com/api/v1/"></script>
<script>
    BX24.init(function() {
        console.log('BX24 initialized successfully.');

        BX24.callMethod('user.current', {}, function(result) {
            if (result.error()) {
                console.error('Error fetching user data: ', result.error());
            } else {
                console.log('User data: ', result.data());
            }
        });
    });
</script>
```

## Response Handling

The function does not return data (`void`). The launch of the handler indicates the result.

## Error Handling

The function has no error codes of its own: it does not call the REST API but waits for data from the parent Bitrix24 page.

#|
|| **Situation** | **What Happens** | **What to Do** ||
|| The page is open outside the application frame — for example, directly in the browser or on your own site | The library does not initialize: on load it throws the exception `Unable to initialize Bitrix24 JS library!`, the `BX24` object becomes `null`, and the handlers do not run | Open the page as a Bitrix24 application ||
|| The library is not connected | Execution stops with the error `BX24 is not defined`. The browser produces the text, so it differs: Safari writes `Can't find variable: BX24` | Connect the library before the first call, see the [Library Overview](../index.md) for details ||
|| Something other than a handler is passed to `BX24.init` | Before initialization, the library ignores an empty value silently; afterwards, a call without a handler produces an error in the console. A value of another type is accepted as a handler, and an error appears in the console when it runs | Pass a function ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-install.md)
- [{#T}](./bx24-install-finish.md)
- [{#T}](./bx24-get-auth.md)
- [{#T}](./bx24-refresh-auth.md)
- [{#T}](../how-to-call-rest-methods/bx24-call-method.md)
- [{#T}](../additional-functions/bx24-ready.md)
