# Set a Function as an Event Handler with BX24.bind

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.bind(element: object, eventName: string, func: callable): void;
```

The `BX24.bind` method assigns the function `func` as a handler of the `eventName` event on the application page element `element`.

In essence, it is a simplified `addEventListener`. For the `mousewheel` and `transitionend` events, the method also subscribes to their variants for older browsers at the same time. The method does not accept `addEventListener` options, such as `capture` or `once`: the handler fires on the element itself and when the event bubbles up from nested elements, but not during the capture phase. If you need these options, call `addEventListener` directly.

The method works on an application page where the [BX24.js library](../index.md) is connected and does not call Bitrix24. The method requires no scope of its own. Call the method when the element is already on the page, for example, in the [BX24.ready](./bx24-ready.md) handler.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **element*** 
[`object`](../../../api-reference/data-types.md) | An application page element, for example, the result of `document.getElementById` ||
|| **eventName*** 
[`string`](../../../api-reference/data-types.md) | The event name without the `on` prefix, for example, `click`. For `mousewheel`, the method additionally assigns the handler to the `DOMMouseScroll` event, and for `transitionend`, to the `webkitTransitionEnd`, `msTransitionEnd`, and `oTransitionEnd` events ||
|| **func*** 
[`callable`](../../../api-reference/data-types.md) | The handler function. It receives the browser event object [(detailed description)](#response). If the function is declared with `function` and is not bound with `Function.prototype.bind`, `this` inside it refers to `element` ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```html
<button id="run-action">Run</button>

<script>
    BX24.ready(function () {
        const button = document.getElementById('run-action');

        BX24.bind(button, 'click', function (event) {
            console.log('Event', event.type, 'on button', this.id); // Event click on button run-action
        });
    });
</script>
```

## Response Handling {#response}

The method does not return data (`void`). When the event occurs, the browser calls `func` and passes it the event object, for example, `MouseEvent` for `click`. The set of fields depends on the event type. They always include the `type` string with the event name and two page elements: `target` holds the element on which the event occurred, and `currentTarget` holds the `element` to which the handler is assigned. They differ if the event came from a nested element.

To remove the handler later with [BX24.unbind](./bx24-unbind.md), store the function in a variable: a handler can be removed only by the same function reference.

## Error Handling

The method does not return error codes.

#|
|| **Situation** | **What Happens** | **What to Do** ||
|| The element is not on the page: `null` is passed in `element`, for example, the result of `getElementById` for a nonexistent `id` | The handler is not assigned, and there is no error | Check the element `id` and call the method after the page is ready, in the [BX24.ready](./bx24-ready.md) handler ||
|| A plain object rather than a page element is passed in `element` | The library writes the handler to a property of the object, for example, `onclick` for the `click` event. The browser event is not tracked in this case, and there is no error | Pass a page element, for example, the result of `document.getElementById` ||
|| No function is passed in `func` | If `func` is not passed or is `null`, the handler is not assigned, and there is no error. If `element` is a page element and a string or a number is passed in `func`, a `TypeError` exception occurs | Pass a function ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-unbind.md)
- [{#T}](./bx24-ready.md)