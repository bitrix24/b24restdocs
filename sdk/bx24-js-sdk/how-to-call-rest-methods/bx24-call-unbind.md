# Call the interface to remove a registered event handler BX24.callUnbind

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

```js
BX24.callUnbind(
    String event,
    String handler[,
        Integer|null auth_type[,
            Function callback
        ]
    ]
);
```

The function `BX24.callUnbind` removes an [event](../../../api-reference/common/events/index.md) handler registered for the current application. The library calls the [event.unbind](../../../api-reference/events/event-unbind.md) method on behalf of the user who opened the application. The spelling `BX24.callUnBind` is the same call, kept for compatibility.

Only the handlers of the application the call is made from are removed, and only the handlers of online events. A handler of an [offline event](../../../api-reference/events/offline-events.md) is removed with the [event.unbind](../../../api-reference/events/event-unbind.md) method and the `event_type=offline` parameter: the function has no parameter that switches the event type.

The function can be called only from an application embedded in the Bitrix24 frame.

If you call the function before [BX24.init](../system-functions/bx24-init.md), the library waits for the initialization and repeats the call, but synchronously returns `false`. Once the request is sent, the function returns `undefined`, so the strict check `=== false` tells a refusal from a sent request only inside the `BX24.init` handler.

{% note info %}

The function works only for a user with Bitrix24 administrator rights. For all other users, it synchronously returns `false`, sends no request, and does not invoke the `callback`. The restriction comes from the library: the [event.unbind](../../../api-reference/events/event-unbind.md) method is available to any user, but BX24.js checks the permissions before sending the request.

{% endnote %}

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../../api-reference/data-types.md) | Event name, for example `ONAPPUNINSTALL`. The value is case-insensitive ||
|| **handler***
[`string`](../../../api-reference/data-types.md) | Address of the event handler. The value is compared exactly, including the scheme and the trailing slash ||
|| **auth_type**
[`integer`](../../../api-reference/data-types.md) | Identifier of the user on whose behalf the event handler is authorized. By default, the parameter is not passed.

Choose the value according to how the handler was registered with [BX24.callBind](./bx24-call-bind.md):

- `0` or an empty value — handlers registered without `auth_type`, that is, with the authorization of the user whose actions triggered the event
- `15` or another user identifier — handlers registered with the `auth_type` of that user
- `null` or an omitted parameter — handlers of the event with any `auth_type` value

The parameters are positional: to remove the handlers with any `auth_type` value and receive the result, pass `null` as the third parameter ||
|| **callback**
[`function`](../../../api-reference/data-types.md) | Function that handles the result of the method call [(detailed description)](#response) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Remove all handlers of an event registered for an address, regardless of `auth_type`:

```js
BX24.init(() => {
    BX24.callUnbind('ONAPPUNINSTALL', 'https://www.my-domain.com/handler/');
});
```

Remove the handlers registered without `auth_type` and process the result:

```js
BX24.init(() => {
    BX24.callUnbind(
        'ONAPPUNINSTALL',
        'https://www.my-domain.com/handler/',
        0,
        (result) => {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log('Handlers removed: ' + result.data().count);
            }
        }
    );
});
```

## Response Handling {#response}

The `callback` function receives an `ajaxResult` object — the same one as in [BX24.callMethod](./bx24-call-method.md#ajax-result). The `data()` method returns the response data, and the `error()` method returns the error.

```json
{
    "count": 1
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **count**
[`integer`](../../../api-reference/data-types.md) | Number of the removed event handlers. The value `0` means that no handler matched the conditions of the call ||
|#

## Error Handling

```json
{
    "status": 400,
    "ex": {
        "error": "ERROR_ARGUMENT",
        "error_description": "Argument 'HANDLER' is null or empty",
        "argument": "HANDLER"
    }
}
```

The error code is available as `result.error().ex.error`, the message as `result.error().ex.error_description`, and the name of the parameter that caused the error as `result.error().ex.argument`. The HTTP status is returned by `result.error().status` and by the `getStatus()` method, while the `toString()` method returns a ready string with the code, the message, and the status.

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_ARGUMENT` | Argument 'EVENT' is null or empty | The event name is not passed ||
|| `400` | `ERROR_ARGUMENT` | Argument 'HANDLER' is null or empty | The handler address is not passed ||
|#

Errors of the `event.unbind` method about missing administrator rights never arrive through this function — the request does not reach the server.

Responses with a `5xx` status do not reach the `callback` either. The library throws a `Query error!` exception from the asynchronous request handler, so a `try/catch` around the call does not intercept it.

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue exploring

- [{#T}](./index.md)
- [{#T}](./bx24-call-method.md)
- [{#T}](./bx24-call-batch.md)
- [{#T}](./bx24-call-bind.md)
- [{#T}](../../../api-reference/events/event-unbind.md)
