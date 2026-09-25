# Calling REST Methods with BX24.js

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

BX24.js allows you to call Bitrix24 methods from the application interface. The library sends the request on behalf of the user who opened the application and automatically adds OAuth 2.0 authorization data.

The library is not available for external applications and webhooks.

> Quick links: [BX24.js Functions for Calling Methods](#all-pages)

## Getting Started

1. Include the BX24.js library and wait for it to initialize using [BX24.init](../system-functions/bx24-init.md)
2. Select a function from the [BX24.js Functions for Calling Methods](#all-pages) table
3. Prepare the data for the call: the method name and its parameters, a batch of requests, or details about the event handler
4. Pass a handler function to receive the result of the call or the error details

Example: retrieve the data of the user who opened the application using the [user.current](../../../api-reference/user/user-current.md) method. The application needs one of the following scopes for this method: `user`, `user_brief`, or `user_basic`.

```js
BX24.init(() => {
    BX24.callMethod('user.current', {}, (result) => {
        if (result.error())
        {
            // for example: insufficient_scope: The request requires higher privileges than provided by the access token (401)
            console.error(result.error().toString());
            return;
        }

        const user = result.data();
        console.log(user.ID, user.NAME, user.LAST_NAME); // 1 Klaus Weber
    });
});
```

{% include [Note on examples](../../../_includes/examples.md) %}

The handler receives a result object: its `data()` method returns the response data, and its `error()` method returns the error. The other methods of the object are described in the [Response Handling](./bx24-call-method.md#ajax-result) section.

## Key Considerations

- The required scope depends on the method being called. See scope values in the [Permissions](../../../api-reference/scopes/permissions.md) guide
- Data access also depends on the permissions of the user on whose behalf the request is performed
- If you call [`BX24.callMethod`](./bx24-call-method.md), [`BX24.callBatch`](./bx24-call-batch.md), [`BX24.callBind`](./bx24-call-bind.md), or [`BX24.callUnbind`](./bx24-call-unbind.md) before [`BX24.init`](../system-functions/bx24-init.md), the library defers the request until initialization completes
- The library works in both cloud and on-premise Bitrix24
- A [`BX24.callBatch`](./bx24-call-batch.md) batch can contain no more than 50 commands. Bitrix24 does not execute commands beyond the limit. The errors returned in this case are described in the [Error Handling](./bx24-call-batch.md#errors) section of that function
- You can register and remove online event handlers using [`BX24.callBind`](./bx24-call-bind.md) and [`BX24.callUnbind`](./bx24-call-unbind.md)
- The rate of requests to Bitrix24 is limited by the [REST API Limits](../../../settings/performance/limits.md)

## Error Handling {#errors}

The library passes an error returned by Bitrix24 to the same handler. The `result.error()` method returns an error object, or `undefined` if there is no error. `result.error().toString()` returns a string with the code, message, and HTTP status, for example `ERROR_METHOD_NOT_FOUND: Method not found! (404)`.

The following are not passed to the handler:

- a response with a `5xx` status, a network failure, and a response that could not be parsed as JSON. The library throws a `Query error!` exception from the asynchronous request handler, so a `try/catch` around the call does not catch it
- the `expired_token` error. The library refreshes the authorization itself and retries the request, and the handler receives the result of the retry

In a batch of requests, errors work differently: they are returned for each command, and an error for the entire batch is not passed to the handler. For details, see the [BX24.callBatch](./bx24-call-batch.md#errors) page.

## BX24.js Functions for Calling Methods {#all-pages}

#|
|| **If you need to** | **Open the page** ||
|| Execute a single Bitrix24 method | [Call a Method with BX24.callMethod](./bx24-call-method.md) ||
|| Handle the data or the error in the response | [BX24.callMethod Response Handling](./bx24-call-method.md#ajax-result) ||
|| Execute several requests in a single batch | [Send a Batch of Requests with BX24.callBatch](./bx24-call-batch.md) ||
|| Register an event handler | [Register a Handler with BX24.callBind](./bx24-call-bind.md) ||
|| Remove a registered event handler | [Remove a Handler with BX24.callUnbind](./bx24-call-unbind.md) ||
|#
