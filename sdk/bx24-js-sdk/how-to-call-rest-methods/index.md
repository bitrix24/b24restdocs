# Calling REST Methods with BX24.js

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

BX24.js allows you to call Bitrix24 methods from the application interface. The library sends the request on behalf of the user who opened the application and automatically adds OAuth 2.0 authorization data.

The library is not available for external applications and webhooks.

> Quick links: [BX24.js Functions for Calling Methods](#all-pages)

## Getting Started

1. Include the BX24.js library and wait for it to initialize using [BX24.init](../system-functions/bx24-init.md)
2. Select a function from the [BX24.js Functions for Calling Methods](#all-pages) table
3. Prepare the data for the call: the method name and its parameters, a batch of requests, or details about the event handler
4. Pass a handler function to receive the result of the call or the error details

## Key Considerations

- The required scope depends on the method being called. See scope values in the [Permissions](../../../api-reference/scopes/permissions.md) guide
- Data access also depends on the permissions of the user on whose behalf the request is performed
- If you call [`BX24.callMethod`](./bx24-call-method.md) or [`BX24.callBatch`](./bx24-call-batch.md) before `BX24.init`, the library defers the request until initialization completes
- In the on-premise version of Bitrix24, use [`BX.rest.callMethod()`](./bx24-call-method.md) instead of [`BX24.callMethod()`](./bx24-call-method.md)
- You can register and remove online event handlers using [`BX24.callBind`](./bx24-call-bind.md) and [`BX24.callUnbind`](./bx24-call-unbind.md)

## BX24.js Functions for Calling Methods {#all-pages}

#|
|| **If you need to** | **Open the page** ||
|| Execute a single Bitrix24 method | [Call a Method with BX24.callMethod](./bx24-call-method.md) ||
|| Handle the data or the error in the response | [Handling the Request Result](./bx24-call-method.md#ajax-result) ||
|| Execute several requests in a single batch | [Send a Batch of Requests with BX24.callBatch](./bx24-call-batch.md) ||
|| Register an event handler | [Register a Handler with BX24.callBind](./bx24-call-bind.md) ||
|| Remove a registered event handler | [Remove a Handler with BX24.callUnbind](./bx24-call-unbind.md) ||
|#
