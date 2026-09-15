# Handle the First Application Launch for a User BX24.install

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.install(someCallback: function | string): void;
```

The `BX24.install` function registers a handler for the event "the application is launched for the first time by the current user". The event occurs immediately after the "library is ready for use" event, but before the handlers set in [BX24.init](./bx24-init.md) are executed.

The handler must report that the setup is complete by calling [BX24.installFinish](./bx24-install-finish.md). Until it does, the `BX24.init` handlers do not run.

The first-launch mechanism is needed by an application with an interface. An application that uses only the API does not need it — see [BX24.installFinish](./bx24-install-finish.md) for details.

The first launch for a user is not the same as installing the application. For an application with an interface, the installation is performed by the installation page — the one specified in the application settings. Only an administrator and a user with the right to install applications can open it, everyone else sees a message that the application is not installed. The installation status is checked with the [app.info](../../../api-reference/common/system/app-info.md) method.

The `BX24.install` handler fires for every user and sets the application up for that user. The stages are covered in [BX24.installFinish](./bx24-install-finish.md), and the installation order is described in the [Application Installation](../../../settings/app-installation/index.md) section.

The function works only inside the frame of an [application](../../../settings/app-installation/index.md). It does not need a scope of its own: it does not call the REST API. Permissions are checked in the Bitrix24 methods that the handler itself calls.

## Function Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **someCallback***
```function | string``` | The first-launch handler. It is called without parameters, the library ignores the value it returns and does not await a promise: only the `BX24.installFinish` call reports completion. Instead of a function, it accepts a string — the URL of a js file. The types are described in the [Data Types and Parameter Formats](../../../api-reference/data-types.md) reference.

A call without a parameter is ignored. A value of another type — a number or an object, for example — causes an error in the browser
||
|#

## When the Event Occurs

The event follows these rules:

- Bitrix24 tracks the first launch and reports it to the library during initialization. The handler must survive a repeated launch: it fires for every user, and it may fire again after the application is updated
- there can be several handlers: they run one after another in the order of registration, and each one passes control to the next by calling [BX24.installFinish](./bx24-install-finish.md)
- after the last handler, Bitrix24 records that the application has already been launched by this user
- with no handlers registered, the event does not occur, and the [BX24.init](./bx24-init.md) handlers run immediately

Register the handler when the application page loads — before the library receives data from Bitrix24. A handler registered later does not get into the first-launch chain.

The [BX24.userOption](../options/bx24-user-option-set.md) and [BX24.appOption](../options/bx24-app-option-set.md) functions do not work yet inside the first-launch handler: the library connects them together with the `BX24.init` handlers, that is, after the whole chain. To read or write a setting at this stage, call the [user.option.set](../../../api-reference/common/settings/user-option-set.md) and [app.option.set](../../../api-reference/common/settings/app-option-set.md) methods.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Inside the handler, Bitrix24 methods are called with [BX24.callMethod](../how-to-call-rest-methods/bx24-call-method.md).

```html
<script src="//api.bitrix24.com/api/v1/"></script>
<script>
    // register the handler right when the page loads: later it will not get into the first-launch chain
    BX24.install(function() {
        BX24.callMethod('user.current', {}, function(res) {
            if (res.error()) {
                console.error('Error fetching user data: ', res.error());
            } else {
                console.log('The Hello World application greets you, ' + res.data().NAME + '!');
                // record that the setup is done: the handler must survive a repeated launch
                BX24.callMethod('user.option.set', {options: {greeting_shown: 'Y'}});
            }

            // call installFinish in both branches, otherwise the setup will not complete
            BX24.installFinish();
        });
    });
</script>
```

The same handler can be moved to a separate js file and its URL passed as a string — either absolute or relative to the application page URL. The library connects the file with a `script` tag and executes it instead of calling a function. In this case `BX24.installFinish` must be called by the code of the file itself, but not synchronously on load: the library attaches the chain to the function only after the file has been executed. An early call does not pass control further, and at the installer stage it completes the installation prematurely.

```js
BX24.install('/install.js');
```

## Response Handling

The function returns no data (`void`). It only registers a handler, and the result of the handler's work is determined by its own code.

## Error Handling

The function has no error codes of its own: it does not call the REST API.

#|
|| **Situation** | **What Happens** | **What to Do** ||
|| The handler did not call [BX24.installFinish](./bx24-install-finish.md) | The chain stops: the following handlers and the `BX24.init` handlers do not run, the first launch is not considered complete, and the handler will run again the next time the application is opened | Call `installFinish` in every branch of the handler, including the error branch ||
|| An unhandled exception occurred in the handler | The library shows a browser `alert` with the text `Installation failed!`, the details are written to the console, and the handler chain stops. Only synchronous exceptions are caught this way: an error inside a nested handler leaves no `alert`, and the chain stops silently | Handle errors inside the handler yourself ||
|| The file from the string variant failed to load | The library waits for it indefinitely: the handler chain stops, and there is no error in the interface | Check the file URL and its availability from the browser ||
|| The function is called outside the application frame | The library does not initialize: on load it throws the exception `Unable to initialize Bitrix24 JS library!`, and the `BX24` object becomes `null` | Open the page as a Bitrix24 application ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-init.md)
- [{#T}](./bx24-install-finish.md)
- [{#T}](./bx24-get-auth.md)
- [{#T}](./bx24-refresh-auth.md)
- [{#T}](../options/index.md)
- [{#T}](../../../settings/app-installation/index.md)
