# Set a Function as an Event Handler with BX24.bind

The method `BX24.bind` sets the function `func` as the event handler for the event `eventName` on the page element `element`.

```js
void BX24.bind(DOMNode element, String eventName, Function func)
```

## Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Name** 
`type` | **Description** ||
|| **element*** 
`DOMNode` | The HTML element on the page (DOM element) for which the handler needs to be set ||
|| **eventName*** 
`string` | The name of the event. For `mousewheel`, `DOMMouseScroll` is additionally included. For `transitionend`, `webkitTransitionEnd`, `msTransitionEnd`, and `oTransitionEnd` are additionally included ||
|| **func*** 
`function` | The event handler function ||
|#

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

```js
BX24.init(function () {
    const button = document.getElementById('run-action');

    BX24.bind(button, 'click', function () {
        console.log('Button clicked');
    });
});
```

## Handling the Response

The method does not return any data (`void`).

## Continue Learning

- [{#T}](./bx24-unbind.md)
- [{#T}](./bx24-ready.md)