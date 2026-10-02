# Interface, Navigation, and Context: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% include notitle [iframe context](../../../_includes/app-runs-in-iframe.md) %}

These BX24.js methods manage the interface of an embedded application in Bitrix24. They let you resize the frame, open Bitrix24 pages and the application window, handle events on the application page, and send commands to the messenger and telephony.

> Quick navigation: [all methods](#all-methods)

## How to Choose the Right Method

#|
|| **If needed** | **Use** ||
|| Set the frame size or fit it to the content | [BX24.resizeWindow](./bx24-resize-window.md), [BX24.fitWindow](./bx24-fit-window.md) ||
|| Change the Bitrix24 page title, scroll the page, or reload it | [BX24.setTitle](./bx24-set-title.md), [BX24.scrollParentWindow](./bx24-scroll-parent-window.md), [BX24.reloadWindow](./bx24-reload-window.md) ||
|| Open the application in a popup window and close that window | [BX24.openApplication](./bx24-open-application.md), [BX24.closeApplication](./bx24-close-application.md) ||
|| Open a Bitrix24 page or a chat, start a call | [BX24.openPath](./bx24-open-path.md), [BX24.im.callTo](./bx24-im-call-to.md), [BX24.im.phoneTo](./bx24-im-phone-to.md), [BX24.im.openMessenger](./bx24-im-open-messenger.md), [BX24.im.openHistory](./bx24-im-open-history.md) ||
|| Wait until the application page is ready or check its state | [BX24.ready](./bx24-ready.md), [BX24.isReady](./bx24-is-ready.md) ||
|| Set or remove an event handler on the application page | [BX24.bind](./bx24-bind.md), [BX24.unbind](./bx24-unbind.md), [BX24.proxy](./bx24-proxy.md), [BX24.proxyContext](./bx24-proxy-context.md) ||
|| Find out the interface language, the Bitrix24 address, the page dimensions, or the user's permission to install the application | [BX24.getLang](./bx24-get-lang.md), [BX24.getDomain](./bx24-get-domain.md), [BX24.getScrollSize](./bx24-get-scroll-size.md), [BX24.isAdmin](./bx24-is-admin.md) ||
|| Load a JavaScript file into the application page | [BX24.loadScript](./bx24-load-script.md) ||
|#

## Getting Started

1. Connect the BX24.js library to the application page — the [Library Overview](../index.md) describes how to do this
2. Call the method you need in the [BX24.init](../system-functions/bx24-init.md) handler: by then, the library has already received data from Bitrix24
3. Handle the result: some methods return it immediately, others pass it to a callback function

For example, an application can find out the dimensions of its page and fit the frame to them:

```js
BX24.init(function () {
    const size = BX24.getScrollSize(); // { scrollWidth: 1108, scrollHeight: 1579 }

    if (size.scrollHeight > window.innerHeight) {
        BX24.fitWindow(function (result) {
            console.log('New frame size:', result.width, result.height); // for example, 1108 1579
        });
    }
});
```

## Key Considerations

The methods work only inside the application frame. Some of them must be called after the library is initialized, in the [BX24.init](../system-functions/bx24-init.md) handler; others can be called right away:

#|
|| **Methods** | **Call After BX24.init** | **How the Result Arrives** ||
|| [BX24.resizeWindow](./bx24-resize-window.md), [BX24.fitWindow](./bx24-fit-window.md), [BX24.setTitle](./bx24-set-title.md), [BX24.scrollParentWindow](./bx24-scroll-parent-window.md), [BX24.openPath](./bx24-open-path.md) | Yes | In the callback function ||
|| [BX24.openApplication](./bx24-open-application.md) | Yes | In the callback function, when the window closes ||
|| [BX24.reloadWindow](./bx24-reload-window.md), [BX24.closeApplication](./bx24-close-application.md) | Yes | Never: Bitrix24 does not call the callback function ||
|| [BX24.isAdmin](./bx24-is-admin.md), [BX24.getLang](./bx24-get-lang.md) | Yes: before initialization, the methods return `false` and an empty string | Immediately, as the method's return value ||
|| [BX24.getDomain](./bx24-get-domain.md), [BX24.getScrollSize](./bx24-get-scroll-size.md), [BX24.isReady](./bx24-is-ready.md), [BX24.proxy](./bx24-proxy.md), [BX24.proxyContext](./bx24-proxy-context.md) | No | Immediately, as the method's return value ||
|| [BX24.ready](./bx24-ready.md), [BX24.loadScript](./bx24-load-script.md) | No | In the callback function, without parameters ||
|| [BX24.bind](./bx24-bind.md) | No | Returns no data. The handler receives the browser event object ||
|| [BX24.unbind](./bx24-unbind.md) | No | Returns no data ||
|| [BX24.im.callTo](./bx24-im-call-to.md), [BX24.im.phoneTo](./bx24-im-phone-to.md), [BX24.im.openMessenger](./bx24-im-open-messenger.md), [BX24.im.openHistory](./bx24-im-open-history.md) | Yes | Never: the command goes to Bitrix24, and no response comes back. A successful call means only that the command was sent, not that the call started or the chat opened ||
|#

The methods require no scope of their own: they control the interface and do not call the REST API.

## Error Handling {#errors}

Only [BX24.openPath](./bx24-open-path.md) returns error codes: `PATH_NOT_AVAILABLE` if the path is invalid, and `METHOD_NOT_SUPPORTED_ON_DEVICE` on a phone or tablet. The other methods have no codes, and a failure shows up in one of three ways.

**JavaScript exception.** The application code stops with an error, for example, if you pass `null` to [BX24.setTitle](./bx24-set-title.md).

**Silent failure.** The command is not executed, and the callback function is not called. For example, [BX24.resizeWindow](./bx24-resize-window.md) does not resize the window opened by [BX24.openApplication](./bx24-open-application.md).

**False success.** The callback function is called even though nothing happened. For example, [BX24.scrollParentWindow](./bx24-scroll-parent-window.md) does not scroll the page in a slider.

The "Error Handling" sections on the method pages cover what to do in each situation.

## Interaction with Other Objects

**System Interface of Bitrix24.** The method [BX24.openPath](./bx24-open-path.md) opens pages and object detail forms in the built-in Bitrix24 slider. The path is passed as a relative one, from the root of Bitrix24: for example, `/crm/deal/details/5/` for a deal. The methods [BX24.im.callTo](./bx24-im-call-to.md) and [BX24.im.phoneTo](./bx24-im-phone-to.md) start a call via internal communication and a call to a phone number, while [BX24.im.openMessenger](./bx24-im-open-messenger.md) and [BX24.im.openHistory](./bx24-im-open-history.md) open the messenger window and the message history of a dialog.

**Embedding Locations.** To have the application open in the Bitrix24 interface, for example, in a tab of a CRM detail form, register a handler via [placement.bind](../../../api-reference/widgets/placement-bind.md) and select an appropriate embedding location from the [list of embedding locations](../../../api-reference/widgets/placements.md).

**Application Initialization and Settings.** The application receives the interface language and the user's permissions when the library is initialized — this is described in the [Initialization and Authorization](../system-functions/index.md) section. The [Calling REST Methods](../how-to-call-rest-methods/index.md) section helps you call Bitrix24 methods from the client side, and the [App Configurations](../options/index.md) section helps you retain the user's choice between launches.

## Overview of Methods {#all-methods}

### Window and Frame Management

#|
|| **Method** | **Description** ||
|| [BX24.resizeWindow](./bx24-resize-window.md) | Resizes the frame containing the application ||
|| [BX24.fitWindow](./bx24-fit-window.md) | Stretches the frame to the full width and fits its height to the content ||
|| [BX24.reloadWindow](./bx24-reload-window.md) | Reloads the entire page with the application, not just the frame ||
|| [BX24.setTitle](./bx24-set-title.md) | Changes the title of the Bitrix24 page above the application ||
|| [BX24.openApplication](./bx24-open-application.md) | Opens a pop-up window with the application frame ||
|| [BX24.closeApplication](./bx24-close-application.md) | Closes the window in which the application is open ||
|| [BX24.scrollParentWindow](./bx24-scroll-parent-window.md) | Scrolls the Bitrix24 page to the specified vertical position ||
|#

### Page Events and Call Context

#|
|| **Method** | **Description** ||
|| [BX24.ready](./bx24-ready.md) | Sets an event handler for "DOM structure of the document is ready for use" ||
|| [BX24.isReady](./bx24-is-ready.md) | Indicates whether the DOM structure of the document is ready for use ||
|| [BX24.proxy](./bx24-proxy.md) | Creates a proxy function for calling a function in the required context ||
|| [BX24.proxyContext](./bx24-proxy-context.md) | Returns the original call context inside a proxy function ||
|| [BX24.bind](./bx24-bind.md) | Sets an event handler for a page element ||
|| [BX24.unbind](./bx24-unbind.md) | Removes an event handler from a page element ||
|#

### Environment Data and Resource Loading

#|
|| **Method** | **Description** ||
|| [BX24.isAdmin](./bx24-is-admin.md) | Checks whether the current user can install this application ||
|| [BX24.getLang](./bx24-get-lang.md) | Returns the Bitrix24 interface language code ||
|| [BX24.getDomain](./bx24-get-domain.md) | Returns the address of the Bitrix24 where the application is open ||
|| [BX24.getScrollSize](./bx24-get-scroll-size.md) | Returns the dimensions of the application page ||
|| [BX24.loadScript](./bx24-load-script.md) | Loads and executes JavaScript files ||
|#

### Navigation and Communication

#|
|| **Method** | **Description** ||
|| [BX24.openPath](./bx24-open-path.md) | Opens a Bitrix24 page in the slider ||
|| [BX24.im.callTo](./bx24-im-call-to.md) | Calls a Bitrix24 user via internal communication ||
|| [BX24.im.phoneTo](./bx24-im-phone-to.md) | Calls a phone number ||
|| [BX24.im.openMessenger](./bx24-im-open-messenger.md) | Opens the messenger window or the chat list ||
|| [BX24.im.openHistory](./bx24-im-open-history.md) | Opens the message history window of a dialog ||
|#