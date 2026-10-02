# Disable Function as Event Handler BX24.unbind

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.unbind(element: object, eventName: string, func: callable): void;
```

The `BX24.unbind` method removes the handler `func` of the `eventName` event, assigned by [BX24.bind](./bx24-bind.md), from the page element `element`.

The method works on an application page where the [BX24.js library](../index.md) is connected and does not call Bitrix24. The method requires no scope of its own.

{% note warning "" %}

A handler can be removed only by passing the same function reference that was passed to [BX24.bind](./bx24-bind.md). If you pass a new function, even one with the same code, the handler remains, and there is no error. Therefore, store a function that you will need to remove in a variable or create it with [BX24.proxy](./bx24-proxy.md).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **element*** 
[`object`](../../../api-reference/data-types.md) | The page element from which the handler needs to be removed ||
|| **eventName*** 
[`string`](../../../api-reference/data-types.md) | The event name without the `on` prefix, for example, `click`. For `mousewheel`, the method also removes the handler from the `DOMMouseScroll` event ||
|| **func*** 
[`callable`](../../../api-reference/data-types.md) | The same function that was passed to [BX24.bind](./bx24-bind.md) ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

A handler that fires only once:

```html
<button id="run-action">Run</button>

<script>
    BX24.ready(function () {
        const button = document.getElementById('run-action');

        function onClick() {
            console.log('Button clicked');
            BX24.unbind(button, 'click', onClick);
        }

        BX24.bind(button, 'click', onClick);
    });
</script>
```

## Response Handling

The method does not return data (`void`).

## Error Handling

The method does not return error codes. If there is nothing to remove, the method does nothing.

#|
|| **Situation** | **What Happens** | **What to Do** ||
|| The handler was assigned to the `transitionend` event | The method removes the handler only from `transitionend`. It is not removed from the `webkitTransitionEnd`, `msTransitionEnd`, and `oTransitionEnd` events that [BX24.bind](./bx24-bind.md) added | Remove the handler from each of these events with a separate `BX24.unbind` call ||
|| `null` is passed in `element` | Nothing happens, and there is no error | Pass a page element ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-bind.md)
- [{#T}](./bx24-proxy.md)