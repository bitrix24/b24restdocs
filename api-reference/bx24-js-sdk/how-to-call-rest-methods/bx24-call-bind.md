# Call the interface to register a new event handler callBind

```js
BX24.callBind(
    String event,
    String handler[
        Integer auth_type[
            Function callback
        ]
    ]
);
```

This interface is for the method [event.bind](../../events/event-bind.md), which registers a new handler for an [event](../../common/events/index.md).

{% note info %}

Works only when authorized as a user with **administration access permission**.

{% endnote %}

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event***
[`string`](../../data-types.md) | Name of the event ||
|| **handler***
[`string`](../../data-types.md) | Link to the event handler ||
|| **auth_type**
[`integer`](../../data-types.md) | Identifier of the user under whom the event handler is authorized. By default, the authorization of the user whose actions triggered the event will be used ||
|| **callback**
[`function`](../../data-types.md) | Function that handles the result of the method call ||
|#

## Example

```http
BX24.callBind('OnAppUninstall', 'http://www.my-domain.com/handler/');
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Continue your exploration

- [{#T}](./bx24-call-unbind.md)
- [{#T}](./bx24-call-method.md)
- [{#T}](./bx24-call-batch.md)
- [{#T}](./files.md)