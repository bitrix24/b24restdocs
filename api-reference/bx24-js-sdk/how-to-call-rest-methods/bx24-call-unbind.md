# Call the interface to remove a registered event handler BX24.callUnbind

```js
BX24.callUnbind(
    String event,
    String handler[
        Integer auth_type[
            Function callback
        ]
    ]
);
```

The interface for the method [event.unbind](../../events/event-unbind.md), which removes a registered [event](../../common/events/index.md) handler.

{% note info %}

Works only when authorized as a user with administrative access permission to the account.

{% endnote %}

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **event**
[`string`](../../data-types.md) | Event name ||
|| **handler**
[`string`](../../data-types.md) | Link to the event handler ||
|| **auth_type**
[`integer`](../../data-types.md) | Identifier of the user under which the event handler is authorized. 

{% note info %}

If you need to remove event handlers set with an empty *auth_type* (authorized on behalf of the user who triggered the event), but keep other handlers, specify *auth_type=0* or an empty value for the parameter. If you need to remove event handlers for all users, specify the value *null*.

{% endnote %}

 ||
|| **callback**
[`function`](../../data-types.md) | Function to handle the result of the method call ||
|#

## Example

```js
BX24.callUnbind('OnAppUninstall', 'http://www.my-domain.com/handler/');
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Continue your exploration

- [{#T}](./bx24-call-bind.md)
- [{#T}](./bx24-call-method.md)
- [{#T}](./bx24-call-batch.md)
- [{#T}](./files.md)