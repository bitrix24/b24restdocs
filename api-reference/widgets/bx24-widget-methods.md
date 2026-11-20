# Methods of BX24 SDK for Widgets

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

In widgets, you can use the JS methods `BX24.openApplication`, `BX24.openPath`, and `BX24.closeApplication`. These methods help open the application interface or standard Bitrix24 pages directly from the embedded widget without calling REST methods.

The methods work only in the application frame: call them after the library is initialized, in the callback function of [BX24.init](../../sdk/bx24-js-sdk/system-functions/bx24-init.md). The [BX24.js](../../sdk/bx24-js-sdk/index.md) overview explains how to include the library on the handler page.

For a detailed description of the syntax, parameters, and callback functions, refer to the section [Additional Functions of BX24.js](../../sdk/bx24-js-sdk/additional-functions/index.md).

> Quick navigation: [all methods](#all-methods)

## Open Interface in Slider with BX24.openApplication

Use `BX24.openApplication` when you need to open your application's interface in a separate slider and pass arbitrary parameters to it.

This method is used for application settings, user dialogs, and displaying additional information over the current interface. If the widget interface is limited by the iframe size, the slider opens over the current page and allows you to show a separate application interface without the constraints of the original embedding area.

The method manages only the application interface and does not replace REST API calls.

{% include [Note on Examples](../../_includes/examples.md) %}

```js
BX24.init(function () {
    // the parameters arrive as a JSON string in PLACEMENT_OPTIONS of the handler in the slider;
    // the handler uses view to decide which screen to show
    BX24.openApplication(
        { view: 'settings' },
        function () {
            console.log('Slider closed');
        }
    );
});
```

More details can be found in the method description [BX24.openApplication](../../sdk/bx24-js-sdk/additional-functions/bx24-open-application.md).

## Open Page with BX24.openPath

Use `BX24.openPath` when you need to open a standard Bitrix24 page, such as a CRM card, task, or user profile.

This method allows you to avoid duplicating the standard Bitrix24 interface within the application. Its purpose is navigation within the interface, not retrieving data from the page directly.

To refresh the widget data after the standard page is closed, use the `callback` of the `BX24.openPath` method: it fires when the user closes the slider. [Events](../events/index.md) and [interactive interaction](../../settings/interactivity/index.md) are needed if the data can change without the widget's involvement.

The method does not work on phones and tablets: `callback` receives the `METHOD_NOT_SUPPORTED_ON_DEVICE` error.

The example opens the card of the deal with the identifier `5`. For your own deal, substitute its identifier in the path.

```js
BX24.init(function () {
    BX24.openPath('/crm/deal/details/5/', function (result) {
        if (result.result === 'error') {
            console.log('Failed to open the page:', result.errorCode);
            return;
        }

        console.log('The user closed the slider');
    });
});
```

More details can be found in the method description [BX24.openPath](../../sdk/bx24-js-sdk/additional-functions/bx24-open-path.md).

## Close Application Window with BX24.closeApplication

Use `BX24.closeApplication` when you need to close the application window that was previously opened via `BX24.openApplication`.

This method operates on the interface side and manages only the application window. It is not intended for closing arbitrary standard Bitrix24 sliders.

```js
BX24.init(function () {
    // the <button id="close-window"> button on a page opened via BX24.openApplication
    document.getElementById('close-window').addEventListener('click', function () {
        BX24.closeApplication();
    });
});
```

More details can be found in the method description [BX24.closeApplication](../../sdk/bx24-js-sdk/additional-functions/bx24-close-application.md).

## Overview of Methods {#all-methods}

> Scope: not required — the methods control the interface and do not call the REST API
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [BX24.openApplication](../../sdk/bx24-js-sdk/additional-functions/bx24-open-application.md) | Opens the application interface in a separate slider and passes parameters to it. ||
|| [BX24.openPath](../../sdk/bx24-js-sdk/additional-functions/bx24-open-path.md) | Opens a standard Bitrix24 page in a slider for navigating to ready objects and sections. ||
|| [BX24.closeApplication](../../sdk/bx24-js-sdk/additional-functions/bx24-close-application.md) | Closes the application window opened via `BX24.openApplication`. ||
|#