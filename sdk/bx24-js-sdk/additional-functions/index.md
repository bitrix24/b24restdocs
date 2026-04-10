# List of Additional Methods

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- No content except for the list of methods.

{% endnote %}

{% endif %}

#| 
|| **Method** | **Description** ||
|| [BX24.isAdmin](./bx24-is-admin.md) | Determines whether the current user has permissions to manage applications. ||
|| [BX24.getLang](./bx24-get-lang.md) | Returns the language identifier in the current Bitrix24. ||
|| [BX24.resizeWindow](./bx24-resize-window.md) | Changes the size of the application frame. ||
|| [BX24.fitWindow](./bx24-fit-window.md) | Sets the size of the application frame according to the content dimensions. ||
|| [BX24.reloadWindow](./bx24-reload-window.md) | Reloads the page with the application (the entire page, not just the frame). ||
|| [BX24.setTitle](./bx24-set-title.md) | Sets the page title. ||
|| [BX24.ready](./bx24-ready.md) | Sets the event handler for the "DOM structure is ready" event. ||
|| [BX24.isReady](./bx24-is-ready.md) | Flag indicating that the "DOM structure is ready". ||
|| [BX24.proxy](./bx24-proxy.md) | Similar to BX.proxy. Equivalent to $.proxy, but with one difference: when the function is called again with the same parameters, it will return a reference to the same proxy function that was the result of the first call, rather than a new proxy function. ||
|| [BX24.proxyContext](./bx24-proxy-context.md) | When called from within, the proxy function will return a reference to the original execution context of the proxy function. ||
|| [BX24.bind](./bx24-bind.md) | Sets the function *func* as the event handler for *eventName* of the *element* object. ||
|| [BX24.unbind](./bx24-unbind.md) | Removes the function *func* as the event handler for *eventName* of the *element* object. ||
|| [BX24.getDomain](./bx24-get-domain.md) | Returns the value of `PARAMS.DOMAIN` saved during the SDK library initialization. ||
|| [BX24.getScrollSize](./bx24-get-scroll-size.md) | This function returns the internal dimensions of the application frame. ||
|| [BX24.loadScript](./bx24-load-script.md) | Loads and executes a client-side JavaScript file. ||
|| [Messenger.startVideoCall](./messenger-start-video-call.md) | The current replacement for `BX24.im.callTo` to initiate a video call. ||
|| [Messenger.startPhoneCall](./messenger-start-phone-call.md) | The current replacement for `BX24.im.phoneTo` to call a phone number. ||
|| [Messenger.openChat](./messenger-open-chat.md) | The current replacement for `BX24.im.openMessenger` and `BX24.im.openHistory`: opens a chat, message history, or chat list. ||
|| [BX24.openApplication](./bx24-open-application.md) | This method opens the application. ||
|| [BX24.closeApplication](./bx24-close-application.md) | This method closes the open modal window with the application. ||
|| [BX24.scrollParentWindow](./bx24-scroll-parent-window.md) | This method scrolls the parent window. ||
|| [Outdated Methods](./outdated/index.md) | Outdated methods `BX24.im.*` that remain available for backward compatibility. ||
|#