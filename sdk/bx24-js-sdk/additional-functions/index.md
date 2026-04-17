# Interface, Navigation, and Context: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Additional methods manage the interface of the embedded application in Bitrix24. They allow you to resize frames, open sliders and windows, handle page events, and invoke messenger methods.

> Quick navigation: [all methods](#all-methods)

## How to Choose the Right Method

1. If you need to manage the application window or frame, start with [BX24.resizeWindow](./bx24-resize-window.md), [BX24.fitWindow](./bx24-fit-window.md), [BX24.setTitle](./bx24-set-title.md), [BX24.openApplication](./bx24-open-application.md), and [BX24.closeApplication](./bx24-close-application.md).
2. If you need to open a section of Bitrix24, a chat, or a call from the application interface, use [BX24.openPath](./bx24-open-path.md), [Messenger.openChat](./messenger-open-chat.md), [Messenger.startVideoCall](./messenger-start-video-call.md), or [Messenger.startPhoneCall](./messenger-start-phone-call.md).
3. If you need to wait for the DOM structure of the page to be ready or bind an event handler, use [BX24.ready](./bx24-ready.md), [BX24.isReady](./bx24-is-ready.md), [BX24.bind](./bx24-bind.md), [BX24.unbind](./bx24-unbind.md), [BX24.proxy](./bx24-proxy.md), and [BX24.proxyContext](./bx24-proxy-context.md).
4. If you need to retrieve runtime environment data, check [BX24.isAdmin](./bx24-is-admin.md), [BX24.getLang](./bx24-get-lang.md), [BX24.getDomain](./bx24-get-domain.md), and [BX24.getScrollSize](./bx24-get-scroll-size.md).

## Interaction with Other Objects

**System Interface of Bitrix24.** The method [BX24.openPath](./bx24-open-path.md) utilizes the built-in Bitrix24 slider to open pages and objects. The methods [Messenger.openChat](./messenger-open-chat.md), [Messenger.startVideoCall](./messenger-start-video-call.md), and [Messenger.startPhoneCall](./messenger-start-phone-call.md) launch the system interface elements of Bitrix24's messenger and telephony.

**Embedding Locations.** For scenarios involving embeddings, register a handler via [placement.bind](../../../api-reference/widgets/placement-bind.md) and select an appropriate embedding location from the [list of embedding locations](../../../api-reference/widgets/placements.md). This is particularly important for the methods [BX24.reloadWindow](./bx24-reload-window.md) and [BX24.scrollParentWindow](./bx24-scroll-parent-window.md), which depend on the context of the application's placement.

## Overview of Methods {#all-methods}

### Window and Frame Management

#|
|| **Method** | **Description** ||
|| [BX24.resizeWindow](./bx24-resize-window.md) | Resizes the frame containing the application ||
|| [BX24.fitWindow](./bx24-fit-window.md) | Adjusts the frame size to fit the content ||
|| [BX24.reloadWindow](./bx24-reload-window.md) | Reloads the page with the application (the entire page, not just the frame) ||
|| [BX24.setTitle](./bx24-set-title.md) | Sets the page title ||
|| [BX24.openApplication](./bx24-open-application.md) | Opens the application ||
|| [BX24.closeApplication](./bx24-close-application.md) | Closes the currently open modal window with the application ||
|| [BX24.scrollParentWindow](./bx24-scroll-parent-window.md) | Scrolls the parent window ||
|#

### Page Events and Call Context

#|
|| **Method** | **Description** ||
|| [BX24.ready](./bx24-ready.md) | Sets an event handler for "DOM structure of the document is ready for use" ||
|| [BX24.isReady](./bx24-is-ready.md) | Indicates whether the DOM structure of the document is ready for use ||
|| [BX24.proxy](./bx24-proxy.md) | Returns a proxy function and reuses it when called with the same parameters ||
|| [BX24.proxyContext](./bx24-proxy-context.md) | When called from within a proxy function, it will return a reference to the original execution context of the proxy function ||
|| [BX24.bind](./bx24-bind.md) | Sets the function *func* as the event handler for *eventName* of the *element* object ||
|| [BX24.unbind](./bx24-unbind.md) | Removes the function *func* as the event handler for *eventName* of the *element* object ||
|#

### Environment Data and Resource Loading

#|
|| **Method** | **Description** ||
|| [BX24.isAdmin](./bx24-is-admin.md) | Determines whether the current user has permissions to manage applications ||
|| [BX24.getLang](./bx24-get-lang.md) | Returns the language identifier in the current Bitrix24 ||
|| [BX24.getDomain](./bx24-get-domain.md) | Returns the value of `PARAMS.DOMAIN` saved during SDK library initialization ||
|| [BX24.getScrollSize](./bx24-get-scroll-size.md) | Returns the internal dimensions of the application frame ||
|| [BX24.loadScript](./bx24-load-script.md) | Loads and executes a client-side JavaScript file ||
|#

### Navigation and Communication

#|
|| **Method** | **Description** ||
|| [BX24.openPath](./bx24-open-path.md) | Opens a path within Bitrix24 in the slider ||
|| [Messenger.startVideoCall](./messenger-start-video-call.md) | Initiates a video call from the Bitrix24 interface ||
|| [Messenger.startPhoneCall](./messenger-start-phone-call.md) | Initiates a phone call from the Bitrix24 interface ||
|| [Messenger.openChat](./messenger-open-chat.md) | Opens a chat, message history, or chat list ||
|#