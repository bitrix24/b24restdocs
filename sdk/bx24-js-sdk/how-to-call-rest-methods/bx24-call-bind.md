# Register an Event Handler BX24.callBind

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

```js
BX24.callBind(
    String event,
    String handler[,
        Integer auth_type[,
            Function callback
        ]
    ]
);
```

The `BX24.callBind` function registers an [online event](../../../api-reference/common/events/index.md) handler for the current application. The library calls [event.bind](../../../api-reference/events/event-bind.md) on behalf of the user who opened the application.

The function can be called only from an application embedded in a Bitrix24 frame. If it is called before [BX24.init](../system-functions/bx24-init.md), the library waits for initialization and retries the call, but synchronously returns `false`.

Registered handlers are removed after the application is deleted or updated. Register them again when installing each application version. Events start arriving only after installation is completed using [BX24.installFinish](../system-functions/bx24-install-finish.md).

{% note info %}

The function works only for a user with Bitrix24 administrator permissions. For other users, it synchronously returns `false`, sends no request, and does not call `callback`. This restriction is imposed by the library: [event.bind](../../../api-reference/events/event-bind.md) allows a regular user to register online events with restrictions.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../../api-reference/data-types.md) | Event name, for example, `ONAPPUNINSTALL`. The name is case-insensitive ||
|| **handler***
[`string`](../../../api-reference/data-types.md) | Event handler URL. It must be accessible to external requests from Bitrix24 servers ||
|| **auth_type**
[`integer`](../../../api-reference/data-types.md) | Identifier of the user on whose behalf the event handler is authorized.

A value of `0` means authorization on behalf of the user whose action triggered the event. This is the default value ||
|| **callback**
[`function`](../../../api-reference/data-types.md) | Function that handles the method call result [(detailed description)](#response) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Register a handler authorized on behalf of the user whose action triggered the event:

```js
BX24.init(() => {
    BX24.callBind(
        'ONAPPUNINSTALL',
        'https://www.my-domain.com/handler/'
    );
});
```

Register a handler authorized on behalf of the user with ID `15` and check the result:

```js
BX24.init(() => {
    BX24.callBind(
        'ONAPPUNINSTALL',
        'https://www.my-domain.com/handler/',
        15,
        (result) => {
            if (result.error())
            {
                console.error(result.error());
            }
            else if (result.data() === true)
            {
                console.log('Handler registered');
            }
        }
    );
});
```

## Response Handling {#response}

The `callback` function receives an `ajaxResult` object, the same as [BX24.callMethod](./bx24-call-method.md#ajax-result). The `data()` method returns response data, and `error()` returns the error.

```json
true
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../api-reference/data-types.md) | Handler registration result. `true` means the handler was registered. `false` means the handler URL failed validation ||
|#

## Error Handling

```json
{
    "status": 400,
    "ex": {
        "error": "ERROR_ARGUMENT",
        "error_description": "Argument 'EVENT' is null or empty",
        "argument": "EVENT"
    }
}
```

The error code is available in `result.error().ex.error`, the message in `result.error().ex.error_description`, and the invalid parameter name in `result.error().ex.argument`. The HTTP status is returned by `result.error().status` and `getStatus()`. The `toString()` method returns a formatted string containing the code, message, and status.

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Meaning** ||
|| `400` | `ERROR_ARGUMENT` | Argument 'EVENT' is null or empty | Event name was not passed ||
|| `400` | `ERROR_ARGUMENT` | Argument 'HANDLER' is null or empty | Handler URL was not passed ||
|| `400` | `ERROR_EVENT_NOT_FOUND` | Event not found | The specified event was not found ||
|| `400` | `ERROR_CORE` | Unable to set event handler | The handler could not be registered ||
|#

Errors from `event.bind` about missing administrator permissions are not returned through this function because the request does not reach the server.

Responses with a `5xx` status are not passed to `callback`. The library throws a `Query error!` exception from the asynchronous request handler, so a `try/catch` around the call does not catch it.

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Exploring

- [{#T}](./index.md)
- [{#T}](./bx24-call-unbind.md)
- [{#T}](./bx24-call-method.md)
- [{#T}](./bx24-call-batch.md)
- [{#T}](../../../api-reference/events/event-bind.md)
