# List of Additional Methods

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no content other than the list of methods

{% endnote %}

{% endif %}

#|
|| **Method** | **Description** ||
|| [BX24.isAdmin](./bx24-is-admin.md) | Determines if the current user has permissions to manage applications. ||
|| [BX24.getLang](./bx24-get-lang.md) | Returns the language identifier of the current account. ||
|| [BX24.resizeWindow](./bx24-resize-window.md) | Changes the size of the application frame. ||
|| [BX24.fitWindow](./bx24-fit-window.md) | Sets the size of the application frame according to the content size. ||
|| [BX24.reloadWindow](./bx24-reload-window.md) | Reloads the page with the application (the entire page, not just the frame). ||
|| [BX24.setTitle](./bx24-set-title.md) | Sets the page title. ||
|| [BX24.ready](./bx24-ready.md) | Sets the event handler for "DOM structure is ready". ||
|| [BX24.isReady](./bx24-is-ready.md) | Flag indicating "DOM structure is ready". ||
|| [BX24.proxy](./bx24-proxy.md) | Similar to BX.proxy. Similar to $.proxy, but with one difference: when the function is called again with the same parameters, it will return a reference to the same proxy function that was the result of the first call, not a new proxy function. ||
|| [BX24.proxyContext](./bx24-proxy-context.md) | When called from within, the proxy function will return a reference to the original execution context of the proxy function. ||
|| [BX24.bind](./bx24-bind.md) | Sets the function *func* as the event handler for *eventName* of the *element* object. ||
|| [BX24.unbind](./bx24-unbind.md) | Removes the function *func* as the event handler for *eventName* of the *element* object. ||
|| [BX24.getDomain](./bx24-get-domain.md) | Returns **window.location.host** of the parent window, i.e., returns the address of the *Bitrix24* account. ||
|| [BX24.getScrollSize](./bx24-get-scroll-size.md) | This function returns the inner dimensions of the application frame. ||
|| [BX24.loadScript](./bx24-load-script.md) | Loads and executes a client-side JavaScript file. ||
|| [BX24.im.callTo](./bx24-im-call-to.md) | Makes a call via internal communication. ||
|| [BX24.im.phoneTo](./bx24-im-phone-to.md) | Makes a call to a phone number. ||
|| [BX24.im.openMessenger](./bx24-im-open-messenger.md) | Opens the messenger window. ||
|| [BX24.im.openHistory](./bx24-im-open-history.md) | Opens the history window. ||
|| [BX24.openApplication](./bx24-open-application.md) | This method opens the application. ||
|| [BX24.closeApplication](./bx24-close-application.md) | This method closes the open modal window with the application. ||
|| [BX24.scrollParentWindow](./bx24-scroll-parent-window.md) | This method scrolls the parent window. ||
|#