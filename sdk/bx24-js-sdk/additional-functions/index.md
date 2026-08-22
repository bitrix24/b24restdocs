# Interface, Navigation, and Context: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% include notitle [iframe context](../../../_includes/app-runs-in-iframe.md) %}

Additional methods manage the interface of the embedded application in Bitrix24. They allow you to resize frames, open sliders and windows, handle page events, and invoke messenger methods.

> Quick navigation: [all methods](#all-methods)

## How to Choose the Right Method

1. If you need to manage the application window or frame, start with [BX24.resizeWindow](./bx24-resize-window.md), [BX24.fitWindow](./bx24-fit-window.md), [BX24.setTitle](./bx24-set-title.md), [BX24.openApplication](./bx24-open-application.md), and [BX24.closeApplication](./bx24-close-application.md).
2. If you need to open a section of Bitrix24, start a call, or open a chat from the application interface, use [BX24.openPath](./bx24-open-path.md), [BX24.im.callTo](./bx24-im-call-to.md), [BX24.im.phoneTo](./bx24-im-phone-to.md), [BX24.im.openMessenger](./bx24-im-open-messenger.md), or [BX24.im.openHistory](./bx24-im-open-history.md).
3. If you need to wait for the DOM structure of the page to be ready or bind an event handler, use [BX24.ready](./bx24-ready.md), [BX24.isReady](./bx24-is-ready.md), [BX24.bind](./bx24-bind.md), [BX24.unbind](./bx24-unbind.md), [BX24.proxy](./bx24-proxy.md), and [BX24.proxyContext](./bx24-proxy-context.md).
4. If you need to retrieve runtime environment data, check [BX24.isAdmin](./bx24-is-admin.md), [BX24.getLang](./bx24-get-lang.md), [BX24.getDomain](./bx24-get-domain.md), and [BX24.getScrollSize](./bx24-get-scroll-size.md).
5. If you need to connect an external JavaScript file on the application page, use [BX24.loadScript](./bx24-load-script.md).

## Key Considerations

- The methods work only inside the application frame and are called after [BX24.init](../system-functions/bx24-init.md). The exceptions are [BX24.ready](./bx24-ready.md) and [BX24.loadScript](./bx24-load-script.md): they depend on the readiness of the page, not the library
- Most of the methods do not return data. They send a command to the Bitrix24 interface, and the result arrives in a callback function
- Environment data — [BX24.isAdmin](./bx24-is-admin.md), [BX24.getLang](./bx24-get-lang.md), [BX24.getDomain](./bx24-get-domain.md), [BX24.getScrollSize](./bx24-get-scroll-size.md), [BX24.isReady](./bx24-is-ready.md), [BX24.proxy](./bx24-proxy.md), [BX24.proxyContext](./bx24-proxy-context.md) — is returned immediately, without a callback
- The `BX24.im.*` methods are an exception: they take no callback at all and return no data (`void`). The command is posted to the parent window and nothing comes back, so a successful call means only that the command was sent — not that the call started or the chat opened
- [BX24.openPath](./bx24-open-path.md) does not work in the mobile application. The method reports this and an unavailable path with the codes `METHOD_NOT_SUPPORTED_ON_DEVICE` and `PATH_NOT_AVAILABLE` in the callback. The methods of this section return no other error codes
- The methods require no scope of their own: they manage the interface instead of calling the REST API

## Interaction with Other Objects

**System Interface of Bitrix24.** The method [BX24.openPath](./bx24-open-path.md) opens pages and object detail forms in the built-in Bitrix24 slider. The path is passed as a relative one, from the root of Bitrix24: for example, `/crm/deal/details/5/` for a deal. The methods [BX24.im.callTo](./bx24-im-call-to.md) and [BX24.im.phoneTo](./bx24-im-phone-to.md) start a call via internal communication and a call to a phone number, while [BX24.im.openMessenger](./bx24-im-open-messenger.md) and [BX24.im.openHistory](./bx24-im-open-history.md) open the messenger window and the message history of a dialog.

**Embedding Locations.** For scenarios involving embeddings, register a handler via [placement.bind](../../../api-reference/widgets/placement-bind.md) and select an appropriate embedding location from the [list of embedding locations](../../../api-reference/widgets/placements.md). This is particularly important for the methods [BX24.reloadWindow](./bx24-reload-window.md) and [BX24.scrollParentWindow](./bx24-scroll-parent-window.md), which depend on the context of the application's placement.

**Application Initialization and Settings.** Environment data becomes available after the library is initialized — this is described in the [Initialization and Authorization](../system-functions/index.md) section. The [Calling REST Methods](../how-to-call-rest-methods/index.md) section helps you call Bitrix24 methods from the client side, and the [App Configurations](../options/index.md) section helps you retain the user's choice between launches.

## Overview of Methods {#all-methods}

### Window and Frame Management

#|
|| **Method** | **Description** ||
|| [BX24.resizeWindow](./bx24-resize-window.md) | Resizes the frame containing the application ||
|| [BX24.fitWindow](./bx24-fit-window.md) | Adjusts the frame size to fit the content ||
|| [BX24.reloadWindow](./bx24-reload-window.md) | Reloads the entire page with the application, not just the frame ||
|| [BX24.setTitle](./bx24-set-title.md) | Sets the page title ||
|| [BX24.openApplication](./bx24-open-application.md) | Opens a pop-up window with the application frame ||
|| [BX24.closeApplication](./bx24-close-application.md) | Closes the pop-up window with the application ||
|| [BX24.scrollParentWindow](./bx24-scroll-parent-window.md) | Scrolls the parent window to the specified vertical position ||
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
|| [BX24.isAdmin](./bx24-is-admin.md) | Determines whether the current user has Bitrix24 administrator permissions ||
|| [BX24.getLang](./bx24-get-lang.md) | Returns the language identifier in the current Bitrix24 ||
|| [BX24.getDomain](./bx24-get-domain.md) | Returns the address of the Bitrix24 where the application is open ||
|| [BX24.getScrollSize](./bx24-get-scroll-size.md) | Returns the dimensions of the application frame content ||
|| [BX24.loadScript](./bx24-load-script.md) | Loads and executes a client-side JavaScript file ||
|#

### Navigation and Communication

#|
|| **Method** | **Description** ||
|| [BX24.openPath](./bx24-open-path.md) | Opens a path within Bitrix24 in the slider ||
|| [BX24.im.callTo](./bx24-im-call-to.md) | Calls a Bitrix24 user via internal communication ||
|| [BX24.im.phoneTo](./bx24-im-phone-to.md) | Calls a phone number ||
|| [BX24.im.openMessenger](./bx24-im-open-messenger.md) | Opens the messenger window or the chat list ||
|| [BX24.im.openHistory](./bx24-im-open-history.md) | Opens the message history window of a dialog ||
|#